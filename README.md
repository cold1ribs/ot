[Отчет УП СИС 2026 Коновалова Настя.docx](https://github.com/user-attachments/files/35072109/2026.docx)
[Отчет УП СИС 2026 Коновалова Настя.docx](https://github.com/user-attachments/files/35074102/2026.docx)





        ПР

using System;
using System.Data;
using System.Data.SqlClient;
using System.Windows.Forms;

namespace MasterskayaKukol
{
    public partial class frmReports : Form
    {
        string connString = @"Data Source=.\SQLEXPRESS;Initial Catalog=MasterskayaKukol;Integrated Security=True";
        string viewName;

        public frmReports(string reportTitle, string sqlViewName)
        {
            InitializeComponent();
            this.Text = reportTitle;
            this.viewName = sqlViewName;
        }

        private void frmReports_Load(object sender, EventArgs e)
        {
            try
            {
                using (SqlConnection conn = new SqlConnection(connString))
                {
                    SqlDataAdapter adapter = new SqlDataAdapter($"SELECT * FROM {viewName}", conn);
                    DataTable dt = new DataTable();
                    adapter.Fill(dt);
                    dgvReport.DataSource = dt;
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show("Ошибка формирования отчета: " + ex.Message, "Ошибка", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }
    }
}

















MessageBox.Show("Информационная система мастерской кукол Пандоро\nВерсия: 1.0\nРазработчик: Коновалова\nГод разработки: 2026", "О программе", MessageBoxButtons.OK, MessageBoxIcon.Information);


приито




using System;
using System.Data;
using System.Data.SqlClient;
using System.Windows.Forms;

namespace MasterskayaKukol
{
    public partial class frmTableView : Form
    {
        // Укажи свое имя сервера из SSMS, если оно отличается от .\SQLEXPRESS
        string connString = @"Data Source=.\SQLEXPRESS;Initial Catalog=MasterskayaKukol;Integrated Security=True";
        string tableName;
        SqlDataAdapter adapter;
        DataTable table;

        public frmTableView(string title, string sqlTableName)
        {
            InitializeComponent();
            this.Text = title;
            this.tableName = sqlTableName;
        }

        private void frmTableView_Load(object sender, EventArgs e)
        {
            // Подписываем кнопки на русском на случай, если в свойствах остались старые имена
            btnAdd.Text = "Добавить";
            btnEdit.Text = "Редактировать";
            btnDelete.Text = "Удалить";

            // Разграничение прав доступа для Менеджера
            if (frmLogin.UserRole == "Менеджер")
            {
                dgvData.AllowUserToDeleteRows = false;
                btnDelete.Enabled = false;

                if (tableName == "Doll_Types" || tableName == "Materials" || tableName == "Masters")
                {
                    dgvData.ReadOnly = true;
                    btnAdd.Enabled = false;
                    btnEdit.Enabled = false;
                    btnSave.Enabled = false;
                }
            }

            LoadData();
        }

        private void LoadData()
        {
            try
            {
                using (SqlConnection conn = new SqlConnection(connString))
                {
                    adapter = new SqlDataAdapter($"SELECT * FROM {tableName}", conn);
                    SqlCommandBuilder builder = new SqlCommandBuilder(adapter);
                    table = new DataTable();
                    adapter.Fill(table);

                    dgvData.Columns.Clear();
                    dgvData.AutoGenerateColumns = true;
                    dgvData.DataSource = table;

                    // Прячем столбец с автоинкрементным ID, чтобы пользователь случайно не менял его руками
                    if (dgvData.Columns.Count > 0)
                        dgvData.Columns[0].ReadOnly = true;

                    // Если открыт Журнал заказов — подменяем цифры на выпадающие списки с ФИО и типом куклы
                    if (tableName == "Orders")
                    {
                        ReplaceWithComboBox("ID_Client", "ФИО клиента", "SELECT ID_Client, Full_Name FROM Clients", "Full_Name", "ID_Client", conn);
                        ReplaceWithComboBox("ID_Master", "ФИО кукольника", "SELECT ID_Master, Full_Name FROM Masters", "Full_Name", "ID_Master", conn);
                        ReplaceWithComboBox("ID_Doll_Type", "Тип куклы", "SELECT ID_Doll_Type, Type_Name FROM Doll_Types", "Type_Name", "ID_Doll_Type", conn);
                    }
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show("Ошибка загрузки данных: " + ex.Message, "Ошибка БД", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }

        private void ReplaceWithComboBox(string colName, string headerText, string query, string displayMember, string valueMember, SqlConnection conn)
        {
            // Ищем столбец без учета регистра букв
            DataGridViewColumn targetCol = null;
            foreach (DataGridViewColumn col in dgvData.Columns)
            {
                if (col.Name.Equals(colName, StringComparison.OrdinalIgnoreCase))
                {
                    targetCol = col;
                    break;
                }
            }

            if (targetCol == null) return;

            int index = targetCol.Index;
            string realDataProperty = targetCol.DataPropertyName;
            dgvData.Columns.Remove(targetCol);

            SqlDataAdapter refAdapter = new SqlDataAdapter(query, conn);
            DataTable refTable = new DataTable();
            refAdapter.Fill(refTable);

            DataGridViewComboBoxColumn combo = new DataGridViewComboBoxColumn();
            combo.Name = realDataProperty;
            combo.HeaderText = headerText;
            combo.DataPropertyName = realDataProperty;
            combo.DataSource = refTable;
            combo.DisplayMember = displayMember;
            combo.ValueMember = valueMember;
            combo.DisplayStyle = DataGridViewComboBoxDisplayStyle.DropDownButton;
            combo.FlatStyle = FlatStyle.Flat;

            dgvData.Columns.Insert(index, combo);
        }

        // Кнопка "Добавить"
        private void btnAdd_Click(object sender, EventArgs e)
        {
            if (dgvData.ReadOnly) return;

            dgvData.Focus();
            int newRowIndex = dgvData.Rows.Count - 1;
            if (newRowIndex >= 0 && dgvData.Columns.Count > 1)
            {
                dgvData.CurrentCell = dgvData.Rows[newRowIndex].Cells[1];
                dgvData.BeginEdit(true);
            }
        }

        // Кнопка "Редактировать"
        private void btnEdit_Click(object sender, EventArgs e)
        {
            if (dgvData.ReadOnly) return;

            if (dgvData.CurrentCell != null && !dgvData.CurrentCell.ReadOnly)
            {
                dgvData.Focus();
                dgvData.BeginEdit(true);
            }
            else
            {
                MessageBox.Show("Выберите ячейку для редактирования!", "Внимание", MessageBoxButtons.OK, MessageBoxIcon.Warning);
            }
        }

        // Кнопка "Удалить" (сразу удаляет и сохраняет в БД)
        private void btnDelete_Click(object sender, EventArgs e)
        {
            if (dgvData.CurrentRow == null || dgvData.CurrentRow.IsNewRow)
            {
                MessageBox.Show("Выберите строку для удаления!", "Внимание", MessageBoxButtons.OK, MessageBoxIcon.Warning);
                return;
            }

            if (MessageBox.Show("Удалить выбранную запись из базы данных?", "Подтверждение", MessageBoxButtons.YesNo, MessageBoxIcon.Question) == DialogResult.Yes)
            {
                dgvData.Rows.Remove(dgvData.CurrentRow);
                SaveToDatabase(false);
            }
        }

        // Кнопка "Сохранить изменения в БД"
        private void btnSave_Click(object sender, EventArgs e)
        {
            SaveToDatabase(true);
        }

        private void SaveToDatabase(bool showMessage)
        {
            try
            {
                // Завершаем редактирование текущей ячейки, чтобы данные попали в таблицу
                dgvData.EndEdit();
                this.BindingContext[table].EndCurrentEdit();

                using (SqlConnection conn = new SqlConnection(connString))
                {
                    adapter = new SqlDataAdapter($"SELECT * FROM {tableName}", conn);
                    SqlCommandBuilder builder = new SqlCommandBuilder(adapter);
                    adapter.Update(table);

                    // Перезагружаем таблицу, чтобы подтянуть новый ID и дату сборки от триггера
                    LoadData();

                    if (showMessage)
                        MessageBox.Show("Данные успешно сохранены в базу данных!", "Готово", MessageBoxButtons.OK, MessageBoxIcon.Information);
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show("Ошибка при сохранении в БД:\n" + ex.Message, "Ошибка", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }
    }
}





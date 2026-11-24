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












итог 

Сборка справочной системы в htm2chm

1. Открой свою папку с проектом и создай внутри нее папку Help_01_Фамилия (подставь свою фамилию).
2. Сохрани в эту папку те 5 HTML-файлов, которые мы делали ранее (index.html, guide.html, functions.html, faq.html, support.html).
3. Запусти программу htm2chm:
	• Нажми File → New Project (или кнопку создания нового проекта).
	• Укажи папку Help_01_Фамилия и выбери главной страницей index.html.
	• Программа автоматически создаст файлы оглавления (.hhc) и указателя (.hhk).
	• Нажми кнопку Compile (Скомпилировать) — в папке Help_01_Фамилия появится готовый файл справки Help_01_Фамилия.chm.
4. Важно для работы справки из программы: скопируй готовый файл Help_01_Фамилия.chm в папку с твоим скомпилированным приложением (VS_01_Фамилия\MasterskayaKukol\bin\Debug\), чтобы при нажатии в меню «Справка → Руководство пользователя» он сразу открывался.

Шаг 2. Создание проекта инсталлятора в Smart Install Maker

1. Сначала убедись, что в Visual Studio ты сделал последнюю сборку проекта (Сборка → Собрать решение / Ctrl + Shift + B), чтобы в папке bin\Debug лежал свежий MasterskayaKukol.exe.
2. Запусти программу Smart Install Maker:
	• Вкладка «Информация»:
		• Название программы: ИС Мастерская кукол «Пандоро»
		• Версия: 1.0
		• Автор: Студент [Твоя Фамилия И.О.]
	• Вкладка «Файлы»:
		• Нажми Добавить файл (+) и выбери из папки bin\Debug файл MasterskayaKukol.exe (а также файлы .config или .dll, если они там есть) и твой файл справки Help_01_Фамилия.chm.
	• Вкладка «Ярлыки»:
		• Добавь создание ярлыка для MasterskayaKukol.exe на Рабочем столе и в меню Пуск.
3. Сохранение проекта и сборка (по требованию методички):
	• В верхнем меню нажми Файл → Сохранить проект как... и сохрани его в главную папку под именем SMM_01_Фамилия.smm (именно этот файл .smm требуется сдать по примечанию в методичке).
	• Нажми кнопку Собрать (или F9), чтобы рядом создался готовый установочный файл setup.exe.

Шаг 3. Финальный коммит в Git и проверка папки сдачи

1. В Visual Studio зайди во вкладку Изменения Git, напиши Финальная сборка приложения и подключение справки и нажми Зафиксировать все, после чего закрой Visual Studio.
2. Переименуй папку с проектом Visual Studio в VS_01_Фамилия.
3. Проверь содержимое главной папки РкИС_Задание_№01_Фамилия перед сдачей преподавателю — внутри должны лежать все 6 элементов:

Имя файла / папки	Что должно быть внутри
VS_01_Фамилия	Папка с проектом Visual Studio, готовым .exe в bin\Debug и скрытой папкой .git с историей коммитов
ТЗ_01_Фамилия.docx	Документ Word с Техническим заданием (по шаблону) и проектом интерфейса (роли, формы, схема переходов)
СхемаБД_01_Фамилия.vsdx	Файл с ER-диаграммой (или файл из ERwin / Visio / картинка схемы)
MasterskayaKukol_01_Фамилия.sql	Выгруженный из SSMS скрипт создания БД MasterskayaKukol (схема + данные + представления + запросы)
Help_01_Фамилия	Папка со всеми 5 .html файлами, файлами .hhc, .hhk и скомпилированным .chm
SMM_01_Фамилия.smm	Сохраненный проект инсталлятора из Smart Install Maker (и собранный рядом setup.exe)

На этом все этапы задания полностью выполнены!

html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="windows-1251">
    <title>Главная — ИС Мастерская кукол «Пандоро»</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; line-height: 1.5; }
        h1 { color: #2c3e50; border-bottom: 2px solid #3498db; padding-bottom: 5px; }
        ul { background: #f8f9fa; padding: 15px 30px; border-radius: 5px; }
    </style>
</head>
<body>
    <h1>Информационная система мастерской кукол «Пандоро»</h1>
    <p>Данная справочная система содержит руководство по работе с автоматизированной информационной системой учета заказов мастерской авторских кукол и игрушек.</p>
    <h2>Разделы справки:</h2>
    <ul>
        <li><a href="guide.html">Руководство пользователя</a></li>
        <li><a href="functions.html">Описание функций системы</a></li>
        <li><a href="faq.html">Часто задаваемые вопросы (FAQ)</a></li>
        <li><a href="support.html">Техническая поддержка</a></li>
    </ul>
</body>
</html>
Используйте код с осторожностью.

2. Файл guide.html (Руководство пользователя)

html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="windows-1251">
    <title>Руководство пользователя</title>
    <style>body { font-family: Arial, sans-serif; margin: 20px; line-height: 1.5; }</style>
</head>
<body>
    <h1>Руководство пользователя</h1>
    <h2>1. Вход в систему</h2>
    <p>При запуске программы открывается окно авторизации. Выберите свою роль и введите пароль:</p>
    <ul>
        <li><b>Администратор</b> (пароль: <code>admin</code>) — полный доступ ко всем справочникам, ценам, заказам и отчетам.</li>
        <li><b>Менеджер</b> (пароль: <code>1234</code>) — оформление заказов и просмотр каталогов без права удаления записей и изменения цен.</li>
    </ul>
    <h2>2. Навигация в главном окне</h2>
    <p>Программа построена на базе многодокументного интерфейса (MDI). В верхней части экрана расположено главное меню:</p>
    <ul>
        <li><b>Справочники</b> — просмотр и редактирование списков клиентов, кукольников, типов кукол и материалов.</li>
        <li><b>Заказы</b> — работа с журналом заказов.</li>
        <li><b>Отчеты</b> — формирование экранных и печатных отчетов.</li>
    </ul>
    <p><a href="index.html">← На главную</a></p>
</body>
</html>
Используйте код с осторожностью.

3. Файл functions.html (Описание функций)

html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="windows-1251">
    <title>Описание функций</title>
    <style>body { font-family: Arial, sans-serif; margin: 20px; line-height: 1.5; }</style>
</head>
<body>
    <h1>Описание основных функций системы</h1>
    <ul>
        <li><b>Учет клиентов и кукольников:</b> хранение ФИО, контактных данных и специализации мастеров.</li>
        <li><b>Ведение каталогов:</b> управление списком типов кукол (уровень сложности и стоимость работы) и материалов (ткани, фурнитура, цена за единицу и остаток на складе).</li>
        <li><b>Автоматический расчет даты промежуточной сборки:</b> вычисляется триггером базы данных на основе даты приема заказа и сложности выбранного типа куклы.</li>
        <li><b>Автоматический расчет стоимости заказа:</b> суммирует стоимость всех выбранных материалов (количество × цена за единицу) и стоимость работы кукольника.</li>
        <li><b>Формирование отчетности:</b> вывод отчета по выполненным заказам и их стоимости, отчета по загруженности кукольников и ведомости расхода материалов.</li>
    </ul>
    <p><a href="index.html">← На главную</a></p>
</body>
</html>
Используйте код с осторожностью.

4. Файл faq.html (Часто задаваемые вопросы)

html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="windows-1251">
    <title>Часто задаваемые вопросы (FAQ)</title>
    <style>body { font-family: Arial, sans-serif; margin: 20px; line-height: 1.5; }</style>
</head>
<body>
    <h1>Часто задаваемые вопросы (FAQ)</h1>
    <h3>Как отметить, что заказ выполнен?</h3>
    <p>Откройте журнал заказов, укажите фактическую дату выполнения заказа в столбце Completion_Date и нажмите «Сохранить изменения в БД». Статус заказа автоматически изменится на «Выполнен».</p>
    <h3>Почему я не могу изменить цену материала или удалить запись?</h3>
    <p>Редактирование цен в каталогах и удаление записей доступно только пользователям с правами <b>Администратора</b>.</p>
    <h3>Как рассчитывается итоговая стоимость заказа?</h3>
    <p>Стоимость рассчитывается автоматически в представлении базы данных путем сложения стоимости всех материалов заказа и стоимости работы кукольника.</p>
    <p><a href="index.html">← На главную</a></p>
</body>
</html>
Используйте код с осторожностью.

5. Файл support.html (Техническая поддержка)

html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="windows-1251">
    <title>Техническая поддержка</title>
    <style>body { font-family: Arial, sans-serif; margin: 20px; line-height: 1.5; }</style>
</head>
<body>
    <h1>Техническая поддержка</h1>
    <p>При возникновении ошибок подключения к базе данных MS SQL Server или сбоях в работе программы обратитесь к разработчику системы:</p>
    <ul>
        <li><b>Разработчик:</b> Студент [Твоя Фамилия И.О.]</li>
        <li><b>Электронная почта:</b> support@pandoro-dolls.ru</li>
        <li><b>Режим работы поддержки:</b> Пн–Пт с 09:00 до 18:00</li>
    </ul>
    <p><a href="index.html">← На главную</a></p>
</body>
</html>







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





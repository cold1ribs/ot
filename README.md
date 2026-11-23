[Отчет УП СИС 2026 Коновалова Настя.docx](https://github.com/user-attachments/files/35072109/2026.docx)
[Отчет УП СИС 2026 Коновалова Настя.docx](https://github.com/user-attachments/files/35074102/2026.docx)

DELETE FROM Order_Materials;
DELETE FROM Orders;
DELETE FROM Clients;
DELETE FROM Masters;
DELETE FROM Doll_Types;
DELETE FROM Materials;

DBCC CHECKIDENT ('Order_Materials', RESEED, 0);
DBCC CHECKIDENT ('Orders', RESEED, 0);
DBCC CHECKIDENT ('Clients', RESEED, 0);
DBCC CHECKIDENT ('Masters', RESEED, 0);
DBCC CHECKIDENT ('Doll_Types', RESEED, 0);
DBCC CHECKIDENT ('Materials', RESEED, 0);




-- 1. Клиенты (5 записей)
INSERT INTO Clients (full_name, phone, email) VALUES 
(N'Смирнова Елена Викторовна', N'+7(911)123-45-67', N'smirnova@mail.ru'),
(N'Козлов Дмитрий Андреевич', N'+7(921)987-65-43', N'kozlov@yandex.ru'),
(N'Морозова Анна Сергеевна', N'+7(905)555-44-33', N'morozova@gmail.com'),
(N'Волкова Ольга Игоревна', N'+7(911)222-33-44', N'volkova@mail.ru'),
(N'Соколов Артём Павлович', N'+7(921)777-88-99', N'sokolov@yandex.ru');

-- 2. Мастера (5 записей)
INSERT INTO Masters (full_name, specialization, phone) VALUES 
(N'Иванова Мария Петровна', N'Шарнирные куклы', N'+7(911)111-22-33'),
(N'Петров Алексей Николаевич', N'Текстильные игрушки', N'+7(911)444-55-66'),
(N'Кузнецова Дарья Олеговна', N'Роспись и ООАК кукол', N'+7(911)777-11-22'),
(N'Новикова Татьяна Сергеевна', N'Пошив одежды для кукол', N'+7(921)333-44-55'),
(N'Белов Максим Викторович', N'Реставрация виниловых кукол', N'+7(905)666-77-88');

-- 3. Типы кукол — популярные форматы (5 записей)
INSERT INTO Doll_Types (type_Name, complexity, work_cost, description) VALUES 
(N'Кукла LOL Surprise', 4, 2500.00, N'Популярная коллекционная кукла-сюрприз с аксессуарами'),
(N'Кукла Барби (Barbie)', 5, 4000.00, N'Классическая шарнирная фэшн-кукла'),
(N'Кукла Monster High', 6, 4500.00, N'Шарнирная кукла с индивидуальным дизайном и росписью'),
(N'Кукла БЖД (BJD)', 9, 15000.00, N'Полиуретановая шарнирная кукла ручной работы'),
(N'Кукла Реборн (Reborn)', 8, 12000.00, N'Реалистичная виниловая кукла с росписью');

-- 4. Материалы (5 записей)
INSERT INTO Materials (material_Name, unit, unit_price, stock_quantity) VALUES 
(N'Хлопок телесный (отрез 50х50)', N'шт.', 450.00, 25),
(N'Глазки стеклянные 12 мм', N'пара', 320.00, 40),
(N'Тресс для волос (натуральный)', N'м', 850.00, 15),
(N'Пуговицы декоративные', N'шт.', 50.00, 100),
(N'Винил для лепки и деталей', N'кг', 1200.00, 20);

-- 5. Заказы (5 записей с учётом assembly_Date)
INSERT INTO Orders (ID_Client, ID_Master, ID_Doll_Type, order_DateTime, assembly_Date, completion_Date) VALUES 
(1, 1, 2, '20261001 10:30:00', '20261019 10:30:00', '20261020 16:00:00'),
(2, 2, 1, '20261015 14:15:00', '20261021 14:15:00', '20261021 12:00:00'),
(3, 3, 3, '20261118 11:00:00', '20261122 11:00:00', NULL),
(4, 4, 4, '20261119 09:45:00', '20261124 14:00:00', '20261125 18:00:00'),
(5, 5, 5, '20261120 15:20:00', '20261126 12:00:00', NULL);


-- 6. Материалы в заказах (5 записей)
INSERT INTO Order_Materials (ID_Order, ID_Material, quantity) VALUES 
(1, 2, 1), 
(2, 3, 2), 
(3, 1, 1), 
(4, 4, 4), 
(5, 5, 2);




USE MasterskayaKukol;
GO

-- Запрос 1: Поиск заказов по ФИО клиента (частичное совпадение)
DECLARE @ClientName NVARCHAR(150) = N'Смирнова';
SELECT * FROM vw_OrdersFullInfo
WHERE [ФИО клиента] LIKE N'%' + @ClientName + N'%';
GO

-- Запрос 2: Выборка заказов по статусу ('В работе' или 'Выполнен')
DECLARE @Status NVARCHAR(50) = N'В работе';
SELECT * FROM vw_OrdersFullInfo
WHERE [Статус заказа] = @Status;
GO

-- Запрос 3: Выборка выполненных заказов за указанный период дат
DECLARE @DateStart DATETIME = '2026-10-01';
DECLARE @DateEnd DATETIME = '2026-10-31';
SELECT * FROM vw_ReportCompletedOrders
WHERE [Дата выполнения заказа] BETWEEN @DateStart AND @DateEnd;
GO

-- Запрос 4: Выборка заказов конкретного кукольника
DECLARE @MasterName NVARCHAR(150) = N'Иванова';
SELECT * FROM vw_OrdersFullInfo
WHERE [ФИО кукольника] LIKE N'%' + @MasterName + N'%';
GO

-- Запрос 5: Поиск материалов с остатком на складе меньше заданного порога
DECLARE @MinStock INT = 30;
SELECT 
    Material_Name AS [Наименование материала],
    Unit AS [Ед. изм.],
    Unit_Price AS [Цена],
    Stock_Quantity AS [Остаток]
FROM Materials
WHERE Stock_Quantity < @MinStock;
GO





using System;
using System.Windows.Forms;

namespace MasterskayaKukol
{
    public partial class frmLogin : Form
    {
        public static string UserRole = ""; // Запоминаем роль для всей программы

        public frmLogin()
        {
            InitializeComponent();
        }

        private void btnLogin_Click(object sender, EventArgs e)
        {
            if (cmbRole.Text == "Администратор" && txtPassword.Text == "admin")
            {
                UserRole = "Администратор";
                this.DialogResult = DialogResult.OK;
                this.Close();
            }
            else if (cmbRole.Text == "Менеджер" && txtPassword.Text == "1234")
            {
                UserRole = "Менеджер";
                this.DialogResult = DialogResult.OK;
                this.Close();
            }
            else
            {
                MessageBox.Show("Неверный пароль или не выбрана роль!", "Ошибка входа", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }
    }
}

using System;
using System.Windows.Forms;

namespace MasterskayaKukol
{
    public partial class frmLogin : Form
    {
        public static string UserRole = "";

        public frmLogin()
        {
            InitializeComponent();
        }

        private void btnLogin_Click(object sender, EventArgs e)
        {
            if (cmbRole.Text == "Администратор" && txtPassword.Text == "admin")
            {
                UserRole = "Администратор";
                this.DialogResult = DialogResult.OK;
                this.Close();
            }
            else if (cmbRole.Text == "Менеджер" && txtPassword.Text == "1234")
            {
                UserRole = "Менеджер";
                this.DialogResult = DialogResult.OK;
                this.Close();
            }
            else
            {
                MessageBox.Show("Неверный пароль или не выбрана роль!", "Ошибка входа", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }
    }
}








static void Main()
{
    Application.EnableVisualStyles();
    Application.SetCompatibleTextRenderingDefault(false);

    frmLogin login = new frmLogin();
    if (login.ShowDialog() == DialogResult.OK)
    {
        Application.Run(new frmMainMDI());
    }
}








using System;
using System.Data;
using System.Data.SqlClient;
using System.Windows.Forms;

namespace MasterskayaKukol
{
    public partial class frmTableView : Form
    {
        // Укажи здесь имя твоего сервера из SSMS (например, localhost или .\SQLEXPRESS)
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
            // Разграничение прав: Менеджер не может удалять записи и менять цены в каталогах
            if (frmLogin.UserRole == "Менеджер")
            {
                dgvData.AllowUserToDeleteRows = false;
                if (tableName == "Doll_Types" || tableName == "Materials" || tableName == "Masters")
                {
                    dgvData.ReadOnly = true;
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
                    dgvData.DataSource = table;
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show("Ошибка загрузки данных: " + ex.Message, "Ошибка БД", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }

        private void btnSave_Click(object sender, EventArgs e)
        {
            try
            {
                using (SqlConnection conn = new SqlConnection(connString))
                {
                    adapter = new SqlDataAdapter($"SELECT * FROM {tableName}", conn);
                    SqlCommandBuilder builder = new SqlCommandBuilder(adapter);
                    adapter.Update(table);
                    MessageBox.Show("Изменения успешно сохранены в базу данных!", "Успех", MessageBoxButtons.OK, MessageBoxIcon.Information);
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show("Ошибка сохранения: " + ex.Message, "Ошибка БД", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }
    }
}

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


OpenChildForm(new frmTableView("Справочник: Клиенты", "Clients"));

4. Повтори то же самое для остальных внутренних пунктов меню:
	• Кукольники → OpenChildForm(new frmTableView("Справочник: Кукольники", "Masters"));
	• Типы кукол → OpenChildForm(new frmTableView("Каталог: Типы кукол", "Doll_Types"));
	• Материалы → OpenChildForm(new frmTableView("Каталог: Материалы", "Materials"));
	• Журнал заказов → OpenChildForm(new frmTableView("Журнал заказов", "Orders"));
	• Выполненные заказы → OpenChildForm(new frmReports("Отчет: Выполненные заказы", "vw_ReportCompletedOrders"));
	• Загруженность кукольников → OpenChildForm(new frmReports("Отчет: Загруженность кукольников", "vw_ReportMastersWorkload"));
	• Расход материалов → OpenChildForm(new frmReports("Отчет: Расход материалов", "vw_ReportMaterialsUsage"));
	• Руководство пользователя → Help.ShowHelp(this, "Help_01_Фамилия.chm");
	• Выход → Application.Exit();
5. И не забудь в самом начале класса frmMainMDI (сразу под InitializeComponent();) добавить сам метод OpenChildForm:csharp
private void OpenChildForm(Form child)
{
    child.MdiParent = this;
    child.Show();
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


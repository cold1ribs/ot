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



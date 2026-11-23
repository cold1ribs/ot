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

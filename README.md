# zostan_programista_dotNet_praca_domowa_T6_l11
T6_l11

create database Enterprise;

CREATE TABLE Products
(
    Id    INT              NOT NULL PRIMARY KEY IDENTITY (1, 1),
    Name  VARCHAR(50)      NOT NULL,
    Price double precision NOT NULL
);

CREATE TABLE Clients
(
    Id          INT         NOT NULL PRIMARY KEY IDENTITY (1, 1),
    FirstName   varchar(20) NOT NULL,
    LastName    varchar(20) NOT NULL,
    City        varchar(50) NULL,
    PhoneNumber varchar(15) NULL,
    Email       varchar(50) NOT NULL,
);

CREATE TABLE Invoices
(
    Id          INT  NOT NULL PRIMARY KEY IDENTITY (1, 1),
    ClientId    INT  NOT NULL,
    CreatedDate DATE NULL DEFAULT GETDATE(),
    Number      AS CONCAT('F', Id, '/', CONVERT(CHAR(10), CreatedDate, 23)),

    CONSTRAINT FK_ClientId FOREIGN KEY (ClientId) REFERENCES Clients (Id)
);

CREATE TABLE InvoicePositions
(
    Id        INT NOT NULL PRIMARY KEY IDENTITY (1, 1),
    InvoiceId INT NOT NULL,
    ProductId INT NOT NULL,
    Quantity  INT NOT NULL,

    CONSTRAINT FK_InvoiceId FOREIGN KEY (InvoiceId) REFERENCES Invoices (Id),
    CONSTRAINT FK_ProductId FOREIGN KEY (ProductId) REFERENCES Products (Id)
);

INSERT INTO Products (Name, Price)
VALUES ('Produkt1', 100.99);
INSERT INTO Products (Name, Price)
VALUES ('Produkt2', 110.99);
INSERT INTO Products (Name, Price)
VALUES ('Produkt3', 10.00);

INSERT INTO Clients
VALUES ('User FN 1', 'User Ln 1', 'City 1', '123123123', 'user1@email.com');
INSERT INTO Clients
VALUES ('User FN 2', 'User Ln 2', 'City 2', '456456456', 'user2@email.com');
INSERT INTO Clients
VALUES ('User FN 3', 'User Ln 3', 'City 3', '789789789', 'user3@email.com');

INSERT INTO Invoices (ClientId)
VALUES (1);
INSERT INTO Invoices (ClientId)
VALUES (2);
INSERT INTO Invoices (ClientId)
VALUES (2);

INSERT INTO InvoicePositions (InvoiceId, ProductId, Quantity)
VALUES (1, 1, 10);
INSERT INTO InvoicePositions (InvoiceId, ProductId, Quantity)
VALUES (2, 2, 20);
INSERT INTO InvoicePositions (InvoiceId, ProductId, Quantity)
VALUES (2, 3, 30);

-- 5a
SELECT i.Id, c.FirstName, c.LastName
FROM Invoices i
         LEFT JOIN Clients c ON i.ClientId = c.Id;

-- 5b
SELECT i.Number, p.Name, p.Price
FROM InvoicePositions ip
         LEFT JOIN Invoices i ON ip.InvoiceId = i.Id
         LEFT JOIN Products p ON ip.ProductId = p.Id;

-- 5c
Select i.Number, sum(ip.Quantity) as [ilosc produktow]
from Invoices i
         left join InvoicePositions ip on i.Id = ip.InvoiceId
GROUP BY i.Number;

-- 5d
SELECT i.Number, SUM(p.Price * ip.Quantity) as [suma na fakturze]
from InvoicePositions ip
         LEFT JOIN Invoices i on I.Id = ip.InvoiceId
         LEFT JOIN Products p on ip.ProductId = P.Id
group by i.Number;



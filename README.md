# Base-de-datos-libreria-
base de datos de una librería, todo el codigo sql server 
use BaseDeDatosEJ
go




create table Miembro (
Id_miembro int primary key not null,
nombre nvarchar (50) not null,
apellido  nvarchar (50) not null,

);




create table Titulo(
Id_titulo int primary key not null,
titulo nvarchar (65) not null,
autor nvarchar (50) not null,
resumen text not null


);
--Cambiar llave primaria Isbn a reserva --
create table Articulo
(
Isbn int primary key identity not null,
Id_titulo int not null,
traduccion nvarchar (25) not null,
portada char (8) not null,
disponible char (1) not null,
FOREIGN KEY (Id_titulo) REFERENCES Titulo(Id_titulo)

);

create table Reservacion
(
Isbn int identity not null,
Id_miembro int not null,
fecha datetime not null,
observaciones nvarchar (55) not null,
primary key(Id_miembro,Isbn),
FOREIGN KEY (Id_miembro) REFERENCES Miembro(Id_miembro),
FOREIGN KEY (Isbn) REFERENCES  Articulo(Isbn)
);

create table Traducciones(
Isbn int not null,
titulo nvarchar (65) not null,
traducciones nvarchar (10) not null
FOREIGN KEY (Isbn) REFERENCES  Articulo(Isbn)
);




insert into Miembro (Id_miembro,nombre,apellido)
values
(1,'Pedro','Sanchez'),
(2,'Maria','Martinez'),
(3,'Pablo','Iglesias'),
(4,'Luis','Bandera'),
(5,'Raul','Castro')

select *from Miembro

insert into Titulo (Id_titulo,titulo,autor,resumen)
values
(123,' Don Quijote de la Mancha','Miguel de Cervantes','Es la primera obra genuinamente desmitificadora de la tradición caballeresca y cortés por su tratamiento burlesco. Representa la primera novela moderna y la primera novela polifónica; como tal, ejerció un enorme influjo en toda la narrativa europea.'),
(423,' Historia de dos ciudades','Charles Dickens','se basan en su mayoría en historias protagonizadas por niños o adolescentes y muestran una intención de reprimenda o denuncia de la sociedad británica de la época. En esta novela histórica se narra la vida en el siglo XVIII, en la época de la Revolución francesa.'),
(926,'El Señor de los Anillos','J. R. R. Tolkien','Su historia se desarrolla en la Tercera Edad del Sol de la Tierra Media, un lugar ficticio poblado por hombres y otras razas antropomorfas como los hobbits, los elfos o los enanos, así como por muchas otras criaturas reales y fantásticas.'),
(937,'El principito','Antoine de Saint-Exupéry','El principito es un cuento poético que viene acompañado de ilustraciones hechas con acuarelas por el mismo Saint-Exupéry. '),
(923,'El hobbit','J. R. R. Tolkien','El Señor de los Anillos y El Silmarillion. Dentro de dicha ficción, el argumento de El hobbit se sitúa en el año 2941 de la Tercera Edad del Sol, y narra la historia del hobbit Bilbo Bolsón, que junto con el mago Gandalf y un grupo de enanos, vive una aventura en busca del tesoro custodiado por el dragón Smaug en la Montaña Solitaria.')

insert into Articulo (Id_titulo,traduccion,portada,disponible)
values
(123,'Chino ingles Español','portada','s'),
(423,'Chino ingles Hindi','portada','n'),
(926,'Chino ingles Árabe','portada','s'),
(937,'Chino ingles','portada','n'),
(923,'Chino ingles Bengalí','portada','s')

select *from Articulo

insert into Reservacion (Id_miembro,fecha,observaciones)
values
(1,7/4/2020,'buena'),
(2,6/8/2020,'mala'),
(3,3/6/2020,'buena'),
(4,15/6/2020,'mala'),
(5,2/8/2020,'buena')



select * from Reservacion
select *from Miembro

Update Miembro set nombre='Frank' where Id_miembro=2 
Update Reservacion set Id_miembro=2 where Isbn=3
select * from Miembro

Update Reservacion set observaciones='buena' where Id_miembro=1 

select nombre from Miembro
where nombre='Pedro' and apellido ='Sanchez'

select nombre from Miembro
where nombre='Pedro' or nombre ='Maria'

SELECT TOP 3 * FROM Miembro;

select nombre, apellido, fecha, traduccion
from Articulo,Miembro,Reservacion
where Articulo.Isbn=Reservacion.Isbn

select * from Miembro
order by  nombre asc,  apellido desc ;

select * from Miembro
order by  nombre desc;

SELECT MIN(Price) AS SmallestPrice
FROM Products;

select
autor,titulo,count(*) as cantidad 
from Titulo
group by autor,titulo


create view v_reservaaa
 as
select nombre,titulo,apellido,autor
from 
((Reservacion
inner join  Miembro on Miembro.Id_miembro=Reservacion.Id_miembro)
inner join Titulo on Titulo.Id_titulo=123 );

go


create view v_reservaaaa
 as
select nombre,titulo,apellido,autor
from 
((Reservacion
inner join  Miembro on Miembro.Id_miembro=Reservacion.Id_miembro)
inner join Titulo on Titulo.Id_titulo=123);

go
select * from v_reservaaa




CREATE PROCEDURE SP_Miembro
@Id_miembro int,
@nombre  nvarchar (50),
@apellido  nvarchar (50)
as
if @Id_miembro is null or @nombre is null or @apellido is null
begin
raiserror ('NO SE PUEDEN INGRESAR VALORES NULOS',6,7)
return
end
if exists (select * from Miembro where Id_miembro=@Id_miembro or nombre=@nombre or apellido=@apellido)
begin
print ('EL REGISTRO YA EXISTE EN LA BASE DE DATOS')
return
end
else 
insert into Miembro values
(@Id_miembro,@nombre,@apellido)
print ('EL REGISTRO SE HA INGRESADO CORRECTAMENTE')
go


exec SP_Miembro 6,',manolo','marco'


create trigger TRDisponible
on Articulo for insert,update
as
set nocount on
declare @disponible char (1)
select @disponible=disponible from inserted

if update (disponible)

begin
if (@disponible!='s' or @disponible!='n')

begin
rollback transaction
raiserror ('no se puede insertar otro valor que no sea s o n',1,5)
return;
end
end


else
begin
if (@disponible!='s' or @disponible!='n')
begin
rollback transaction
raiserror ('no se puede insertar otro valor que no sea s o n',1,5)
return;
end
end

insert into Articulo(Id_titulo,traduccion,portada,disponible)
values (123,'espanol','hola','b')



update Articulo set disponible='c' where Id_titulo=123

select * from Articulo

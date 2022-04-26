# Yulia Daruzhinskaia
## Junior Frontend Developer
### Contacts
* Location: Minsk, Belarus
* Phone: +375 44 51 01 001
* Email: dorujinskaia@gmail.com
* GitHub: Yulia-Dor

## Skills:
1. I started my work with Microsoft Visual Basic 6.0 and Oracle database.
1. Actively studied further work with the PostgreSQL database. Creation of copies of a DB\Restoration. Creation of users with distribution of access rights. Implementation of procedures and functions, writing complex queries. Working with composite types. Implementation of PL/Python functions.
1. Worked on migrating data from Oracle to PostgreSQL.
1. Working with Python Implementation of scripts for processing data from devices (controllers) using TCP, UDP, SNMP, MQTT, SMPP, OCPP protocols.
1. Work in Linux environment. Installing software, running scripts, working with PostgreSQL, basic commands.

## Skills and Proficiency:
* Oracle, PostgreSQL
* Python
* Visual Basic 6.0
* Linux
* Git, GitHub
* HTML5, CSS3
* JavaScript Basics
* VS Code, IntelliJ IDEA

## Code Example:
* Postgres's function. Saving files to the server with a record of information about the storage location. *

```javascript
CREATE OR REPLACE FUNCTION public.save_file_return (
  p_return bigint,
  p_name text
)
RETURNS text AS
$body$
  declare
  loid oid;
  lfd integer;
  filepath text;
  puth text;
  folder text;
  l_id_row bigint;
  l_error text;
  msg text;
  ctx text;
  l_result text;
begin
 begin
  insert into return_file(id_return, id_extension, name_file_fact, f_year, f_month, f_day, id_user, pa)
  (select t.id_return, 
          (select e.id from return_file_extension e where e.name =last_elem(string_to_array(t.file_name, '.'))),
          t.file_name,
          split_part(t.year_month_dem,'/',1),
          split_part(t.year_month_dem,'/',2),
          split_part(t.year_month_dem,'/',3),
          t.user_id,
          1            
    from return_file_tmp t 
    where t.id_return=p_return and t.file_name=p_name) RETURNING id into l_id_row;
    
    update return_file
    set name_file=l_id_row
    where id=l_id_row;
 
  folder= (select f.f_year from return_file f where id=l_id_row )::text||'/'||(select case when f.f_month::INTEGER<10 then '0'||f.f_month else f.f_month end from return_file f where id=l_id_row)::text||'/'||(select case when f.f_day::INTEGER<10 then '0'||f.f_day else f.f_day end from return_file f where id=l_id_row)::text;
  puth='/media/'||folder||'/';
  filepath :=  puth||(select name_file from return_file where id=l_id_row)||'.'||(select (select e.name from return_file_extension e where e.id=f.id_extension) from return_file f where f.id=l_id_row);
  RAISE NOTICE 'filepath %', filepath;
  loid := lo_create(-1);
  lfd  := lo_open(loid, CAST(X'20000' | X'40000' AS integer));
  perform lowrite(lfd,(select  file from return_file_tmp t where t.id_return=p_return and t.file_name=p_name));
  perform lo_export(loid, filepath);
  perform lo_close(lfd);
  perform lo_unlink(loid);
  
  delete from return_file_tmp
   where id_return=p_return
         and file_name=p_name;
         
  if l_id_row is null then
      l_result='Warning! File not found!';
  else
      l_result='OK';
  end if;
  
 EXCEPTION
  WHEN OTHERS  THEN
    l_error=SQLSTATE;
    if l_error='23502' then
        RAISE NOTICE '%', 1;
        l_result='Error! Document extension not found!';
    ELSIF l_error='58P01' then
        RAISE NOTICE '%', 1;
        l_result='Error! Path not found!';
    ELSIF l_error='42501' then
        RAISE NOTICE '%', 1;
        l_result='Error! Permission denied!';
    ELSE 
        l_result=SQLERRM;
    end if;
 end;
  
  return l_result;
  end;
$body$
LANGUAGE 'plpgsql'
VOLATILE
CALLED ON NULL INPUT
SECURITY INVOKER
PARALLEL UNSAFE
COST 100;
```

## Education:
* Molodechno College of Trade and Economics, technician - programmer
* Higher Radio Engineering College, teacher-programmer

## Courses:
* DEV1. Development of the server part of the PostgreSQK application 12. Basic course
* DEV2. Development of the server part of the PostgreSQK application 12. Advanced course
* QPT: PostgreSQL. Query Optimization
* RS School. Preparatory course «JavaScript/Front-end. Stage 0"

## Languages:
* Russian - Native
* English - A2

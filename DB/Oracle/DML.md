- <b>컬럼추가</b>
```sql
ALTER TABLE tblprojectcomment ADD pcregdate date default sysdate not null;  
```  
- **컬럼삭제**
```sql
ALTER TABLE tblqnaanswer DROP COLUMN qasubject;
```
<br>

- **자료형 사이즈**
 ```sql
ALTER TABLE dbo.emp ALTER COLUMN email VARCHAR(100)
```

- **데이터 타입변경**
 ```sql
ALTER TABLE dbo.emp ALTER COLUMN email TEXT
```

<br>

- **NOT NULL 설정**
```sql
ALTER TABLE dbo.emp ALTER COLUMN email VARCHAR(25) NOT NULL
```
- **NOT NULL 제거**
```sql
ALTER TABLE dbo.emp ALTER COLUMN email VARCHAR(25)
```

<br>

- **DEFAULT값 설정**
```sql
ALTER TABLE tblCommuComment ADD CONSTRAINT 
commucomment_default DEFAULT sysdate FOR ccregdate;
```
- **DEFAULT값 제거**
```sql
ALTER TABLE dbo.emp DROP CONSTRAINT df_emp_email
```

<br>

- **컬럼 추가 (NOT NULL 설정)**
```sql
ALTER TABLE dbo.emp ADD email VARCHAR(25) NOT NULL
```
- **컬럼 추가 (DEFAULT 값 설정)**
```sql
ALTER TABLE dbo.emp ADD email VARCHAR(25) DEFAULT 'None'
```
- **컬럼 추가 (NOT NULL 설정 + DEFAULT 값 설정)**
```sql
ALTER TABLE dbo.emp ADD email VARCHAR(25) NOT NULL DEFAULT 'None'
```
- **외래키 컬럼추가**
```sql
ALTER TABLE tblcommunity
ADD CONSTRAINT fk_stuseq foreign KEY(stuseq) references tblstudent (stuseq);
```
- **유니크 제약조건 추가**
```sql
ALTER TABLE 테이블명 ADD CONSTRAINT 제약조건명 UNIQUE(컬럼명);
```
## Join

 두 개 이상의 테이블이나 DB를 연결하여 데이터를 검색하는 방법
 
 테이블을 연결하려면 적어도 하나의 칼럼을 서로 공유하고 있어야 하므로 이를 이용하여 데이터 검색에 활용
 
 ![image](https://user-images.githubusercontent.com/32594290/102319570-74bfe000-3fbe-11eb-8a4c-adf8aead2613.png)


## Join 종류

### INNER JOIN

 ![image](https://user-images.githubusercontent.com/32594290/102319669-93be7200-3fbe-11eb-9b36-3fc7818306ce.png)

교집합으로 기준 테이블과 Join한 테이블의 **중복된 값**을 보여준다.

결과값은 A 테이블과 B테이블이 모두 갖고 있는 데이터만 검색된다.


### LEFT OUTER JOIN

![image](https://user-images.githubusercontent.com/32594290/102324889-ccae1500-3fc5-11eb-9a3a-99770888f948.png)

기준 테이블 값 + 테이블과 기준 테이블의 중복된 값을 보여준다.

왼쪽 테이블 기준으로 JOIN을 진행

결과값은 A 테이블의 모든 데이터와 A 테이블과 B 테이블의 중복되는 값이 검색된다.


### RIGHT OUTER JOIN

![image](https://user-images.githubusercontent.com/32594290/102333392-b3f72c80-3fd0-11eb-8470-1de59de32b32.png)

 LEFT OUTER JOIN의 반대로 오른쪽 테이블 기준으로 JOIN을 진행
 
 결과값은 B 테이블의 모든 데이터와 A 테이블과 B 테이블의 중복되는 값이 검색된다.
 

### FULL OUTER JOIN

 ![image](https://user-images.githubusercontent.com/32594290/102333561-f0c32380-3fd0-11eb-9b33-fa3a18d992b5.png)

합집합으로 생각
 
A 테이블 데이터, B 테이블 데이터 모두 검색


### CROSS JOIN

 ![image](https://user-images.githubusercontent.com/32594290/102333638-0a646b00-3fd1-11eb-86e9-d1cab795f902.png)

크로스 조인은 모든 경우의 수를 전부 표현해주는 방식

기준 테이블이 A일 경우 A의 데이터 한 ROW를 B 테이블의 전체와 JOIN하는 방식

결과값 = N * M 

### SELF JOIN

![image](https://user-images.githubusercontent.com/32594290/102333768-354ebf00-3fd1-11eb-9bd0-8255a22c1b60.png)

셀프 조인은 자기 자신과 자기 자신을 조인

하나의 테이블을 여러번 복사해서 조인한다고 생각. 자신이 가지고 있는 칼럼을 다양하게 변형시켜 활용할 경우 자주 사용
 

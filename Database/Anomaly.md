## Anomaly (이상)

 정규화를 하는 이유는 잘못된 테이블 설계로 인한 Anomaly(이상 현상)가 나타나기 때문이다. 
 
### Anomaly란?
 
 {Student ID, Course ID, Department, Course ID, Grade} 와 같은 테이블이 있다고 가정하자.
 
#### 삽입 이상(Insertion Anomaly)
 
 기본키가 {Student ID, Course ID}일 때, Course를 수강하지 않은 학생은 Course ID가 없는 현상이 발생. 결국 Course ID를 Null로 
 할 수 밖에 없는데, 기본키는 Null이 될 수 없으므로 Table에 추가될 수 없다.
   
 따라서, 굳이 삽입하기 위해 '미수강'과 같은 Course ID를 만들어야 한다.
   
 > **불필요한 데이터를 추가해야만 삽입할 수 있는 상황 = Insertion Anomaly**
   
#### 갱신 이상 (Update Anomaly)
 
 임의의 학생의 전공(Department)이 'Computer' ⇒ 'Music'이 되는 경우
   
 모든 Department를 'Music'으로 바꿔야 한다. 그러나 일부를 바꾸지 못하는 경우 제대로 파악이 불가능하다.
   
 > **일부만 변경하여 데이터의 불일치가 발생하는 현상 = Update Anomaly**
  
#### 삭제 이상 (Deletion Anomaly)

 임의의 학생이 수강을 철회하는 경우, {Student ID, Course ID, Department, Course ID, Grade}의 정보 중 Student ID, Department와 같은
 학생에 대한 정보도 함께 삭제가 된다.
 
 > **Tuple 삭제로 인해 꼭 필요한 데이터까지 함께 삭제되는 현상 = Deletion Anomaly**

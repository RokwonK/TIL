# CollectionView
변경가능하고 유연한 레이아웃을 사용하고 다양한 레이아웃을 보여줄 때 많이 활용됨.  
하나의 CollectionView는 아래의 4개로 이루어진다.  

- Cell
- 보충 뷰(Supplementary views)
- Decoration views
- Layout Object(배치 방법, 시각적 스타일 결정)

### 1. Cell  
컬렉션뷰의 주요 컨텐츠.  
여러개의 Cell로 하나의 CollectionView가 완성됨
CollectionView는 datasource을 구현한 객체에서 표시할 셀의 정보를 가져옴.(구현된 함수에따라 값을 가져옴)  
Cell은 UICollectionViewCell의 클래스의 인스턴스/상속받은 클래스의 인스턴스이다.  

### 2. Supplementary views
셀들을 모아놓은 하나의 View의 제목이나 하단 글 같은 것.

### 3. Decoration views
컬렉션 뷰에 대한 배경을 꾸밀때 사용

### 4. Layout OBject
컬렉션 뷰 내의 아이템 배치, 시각적 스타일을 결정함.(아이템간 간격, 아이템 크기 등 설정 가능함)


<br>

## 필수 메서드들을 알아보자!

### UICollectionViewDatasource
이 프로토콜을 필수적으로 채택하여야 하고 컬렉션 뷰의 컨텐츠를 관리, 표현하기 위한 메서드들이 정의되어 있다.  
아래의 두 메서드는 프로토콜 채택시 필수로 구현해야하는 메서드들이다. 
```swift
//지정된 섹션에 표시할 항목의 개수를 묻는 메서드.
func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int

//컬렉션뷰의 지정된 위치에 표시할 셀을 요청하는 메서드.
func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell
```

### 이 외의 것이 필요할때는 찾아보면서 공부해보자!!!

# Xcode - Target과 Project

## **Target이란**  
Xcode에서 Build를 실행하여 생성되는 최종 제품(end product)  
product를 build하는 방법을 명시함(Build Phases)    
> 여기서 최종 제품이란? - 앱 or 프레임워크 or Unit Test번들  

### **Target의 특징**
- 한 프로젝트에는 하나 이상의 Target이 포함 됨.
- 각 Target은 하나의 제품(product)를 생성함.  
- product building instructions를 포함함
    - Build Settings과 Build Phases를 가진다는 뜻
    - 여기서 Build Settings은 `Project`에도 존재 -> Target은 이 Project의 Build Settings를 상속받음!
    - 즉, Target의 Build Settings는 Override 할 수 있다.
- 한 번에 하나의 Target만 지정가능함.  

### **Target이 생성한 제품이 다른 Target과 연관이 되어 있다면?**  
어떻게 할까?  
- Xcode는 dependency를 발견함, 알아서 필요한 순서대로 제품을 build => implicit dependency(암묵적인 종속성)
- Build Settings에서 명시적으로 target dependency를 지정가능함.  

### **Target과 Configuration의 차이는 뭘까?**  
목적이 다르다  
- Configuration은 다른 버전을 만들 수 있음. (dev, stage, production)
- Target은 최종 제품을 나누어 만든다(유,무료앱으로 / 한국,일본 앱으로)
- 하지만 Configuration이 할 일을 Target으로도 할 수 있음.  



<br><br>



## **Project**  
- 모든 파일, 리소스, software를 `빌드하는데 필요한 정보의 저장소`
- 요소간의 관계가 유지됨.  
- target을 하나 이상 포함함.
- 모든 target에 대한 기본 Build Settings를 정의  

### **Project는 '정보의 저장소'라고 했는데 어떤 정보를 가지고 있을까?**
1. 소스파일 reference
    - 구현파일을 포함한 소스코드
    - Libraries, frameworks, Resource files
    - 이미지 파일, interface Bilder(nib) 파일
2. 프로젝트에 대해 둘 이상의 build configuration을 지정할 수 있다.
3. structure navigator에서 소스코드를 통합하는 그룹
4. 각 Target이 지정하는 목표들
    - 


<br><br>


## **Build Settings**  
빌드 프로세스의 특정 측면을 수행하는 방법에 대한 정보가 들어있음  


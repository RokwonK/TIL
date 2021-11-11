# View의 frame과 bounds의 차이

**frame**은 superview의 좌표계를 기준  
**bounds** 자기 자신의 좌표계를 기준  
으로 위치,크기를 나타냄.  

각각 origin(위치 정보)을 바꾸면 기준 뷰가 아닌 하위뷰가 움직이게 됨.  
즉,  
nowView.frame.origin을 바꾸면 nowView가 움직임.
nowView.bounds.origin을 바꾸면 해당 view의 하위뷰들이 움직임.

**추가**
bounds는 default가 0,0 / scrollView의 핵심이 됨.
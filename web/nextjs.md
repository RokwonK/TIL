---
title: nextjs의 렌더링 방식
author: 김록원
date: 2022.10.01
category: node
---

# Next.js의 렌더링 방식

React는 기본적으로 CSR(Client Side Rendering)을 이용한다. 이는 SEO에 매우 취약한 형태이다.

> 💡 CSR이란?
CSR은 아래와 같은 과정을 거쳐 브라우져에 UI를 띄우게 된다.  
(브라우저) 페이지 요청 -> (리액트 서버)빈 페이지 응답 -> (브라우저)js loads, html 렌더링, 리액트 컴포넌트 만들기

위와 같은 CSR은 처음 브라우저가 빈 페이지를 받기 때문에 SEO에 제대로 잡히지 못한다.  
이에 대한 해결법으로 SSR을 지원하는 Next.js, Remix등의 프레임워크들이 등장하게 되었다.  

<br />
<hr>

## Next.js
Next.js는 React에 SSG(Static-Site Generation)과 SSR(Server Side Rendering)을 도와주는 프레임워크이다.  

<br />

### Next.js의 Pre-Rendering
Pre-Rendering이라 불리는 과정을 통해 리액트 서버에서 미리 html 파일을 구성한 후 브라우저에게 응답해준다. html내에 데이터들이 들어 있기때문에 SEO에 효과적이다.

> 💡 Pre-Rendering?
(브라우저) 페이지 요청 -> (리액트 서버)html 빌드 -> (리액트 서버)만들어진 html 응답 -> (브라우저)리액트 컴포넌트 구성

<br />
<hr>

## Next.js의 Datafetching 기법
위에서 설명했다시피 Next.js는 next를 빌드시에 html build를 실행하게 된다.  
이때 어떤 페이지는 html을 구성하기 위해 데이터가 필요할 수도 있다.  
예를 들어 `{domain}/posts/5`과 같은 페이지는 5번 포스트 정보를 서버에서 가져와야한다.  
Next.js는 **페이지를 구성하기 위한 데이터를 불러오는 방법**에 대해 두가지 Datafetching 기법을 제공한다.

<br />

### SSG(Static-Site Generation)
next build시에 데이터를 불러와서 페이지를 구성할 수 있도록하는 것이다.  
next build 후 데이터가 변동이 없을 경우 사용한다. ex) 포스트 게시글 등

<br />

**getStaticProps**  
- 해당 페이지 안에서 getStaticProps라는 이름의 함수를 export
- html build시 불러와야하는 데이터들을 호출
- props로 data를 넘기면 해당 페이지 컴포넌트에서 props로 받는다.
```jsx
export const getStaticProps = async () => {
  const res = await axios.get(`https://jsonplaceholder.typicode.com/posts`);
  const data = res.data;

  console.log(data[1]); 

  return {
    props: {
      list: data,
    },
  };
};
```  

<br />

**getStaticPaths**  
- 동적 라우팅을 처리하기 위한 메서드이다.  ex) `posts/[id]`
- 동적 라우팅 페이지([id].tsx)는 getStaticPaths 함수를 먼저 실행한다.
- 이때 만들어진 포스트들의 id를 모두 가져와서 paths로 리턴한다.(fallback은 지정되지 않은 경로에 대한 요청에는 404 에러를 출력할 것인지를 묻는 프로퍼티이다) 
- getStaticProps에서는 각 id에 따라 각각 페이지마다 데이터를 받고 post로 props를 보내준다
```jsx
export const getStaticPaths = async () => {
  return {
    paths: [
      { params: { id: "1" } },
      { params: { id: "2" } },
      { params: { id: "3" } },
    ],
    fallback: true,
  };
};

export const getStaticProps = async (ctx) => {
  const id = ctx.params.id;
  const res = await axios.get(
    `https://jsonplaceholder.typicode.com/posts/${id}`
  );
  const data = res.data;

  return {
    props: {
      post: data,
    },
  };
};

export default function Post({ post }) {
  // Render post...
}
```

### SSR(Server Side Rendering)
페이지를 요청했을 때 데이터를 불러와서 페이지를 구성하는 방법이다.  
페이지 요청때마다 데이터를 불러와서 html을 만들기 때문에 SSG에 비해 성능이 많이 떨어진다.  ex) SNS의 피드 페이지 등

<br />

**getServerSideProps**  
- getStaticProps와 사용방법은 같고 이름만 다르다
- 페이지 요청때마다 해당 함수가 실행된다는게 다름!
```js
export const getServerSideProps = async (ctx) => {
  const id = ctx.params.id;
  const res = await axios.get(
    `https://jsonplaceholder.typicode.com/posts/${id}`
  );
  const data = res.data;

  console.log(data); // 해당 콘솔은 어디에서 출력이 되나요?

  return {
    props: {
      item: data,
    },
  };
};
```

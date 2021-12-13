---
title:  "[Web] axios에서 GET, DELETE 요청에 데이터 전달하기"
search: true
categories: 
  - web
tag:
  - Axios
last_modified_at: 2021-12-10T08:06:00-05:00
---

최근에 Spring boot + Vue.js로 웹 프로젝트를 진행했었다.

REST API로 서버와 프론트가 통신하게 했었는데 프론트쪽에서 서버로 `GET`과 `DELETE` 요청을 보낼 때 문제가 생겼다.

```javascript
async function listArticle(param, success, fail) {
  await axios.get(`/board`, JSON.stringify(param)).then(success).catch(fail);
}

async function writeArticle(article, success, fail) {
  await axios.post(`/board`, JSON.stringify(article)).then(success).catch(fail);
}

async function modifyArticle(article, success, fail) {
  await axios.put(`/board`, JSON.stringify(article)).then(success).catch(fail);
}

async function deleteArticle(data, success, fail) {
  await axios.delete(`/board`, JSON.stringify(data)).then(success).catch(fail);
}
```

통신에 사용하는 axois메소드를 `api.js`에 위와 같이 작성해서 필요한 vue 컴포넌트에 임포트해서 사용했었는데

`PUT`과 `POST`는 문제가 없었지만 `GET`과 `DELETE`는 서버에서 body부분 (param, data)을 읽어오지 못하고 에러를 뿜어냈다.

<br/>

## 원인

관련 내용을 찾아보니깐 기본적으로 `GET`과 `DELETE` 메소드는 본문(body)을 지원하지 않는다고 한다.

사용하려면 `GET`은 `params`로 묶어서 보내야하고 `DELETE`는 `data`로 묶어서 보내야 한다.


## 해결방안

굳이 body에 데이터를 넣지 않아도 `GET`이나 `DELETE`는 주소에 쿼리파라미터를 넘겨줘서 해결해줄 수도 있지만 

그러면 RESTful하지 않게 되서 찝찝하다.

![쒸익쒸익짤](https://user-images.githubusercontent.com/47655983/145408229-367d05e3-8242-4e5c-a5c2-fd7a284ad5bf.png)

이왕 만드는거 RESTful하게 만들어보고 싶었다.

아무튼 그래서 쿼리파라미터를 넘기지 않고 데이터를 넘기는 방법을 찾아본 결과

원인에도 써놨듯이 `GET`은 `params`로 묶어서 보내고 `DELETE`는 `data`로 묶어서 보내면 된다는 사실을 알았다!

해결한 코드를 보자

```javascript
async function listArticle(param, success, fail) {
  await axios.get(`/board`, { params: param }).then(success).catch(fail);
}

async function writeArticle(article, success, fail) {
  await axios.post(`/board`, JSON.stringify(article)).then(success).catch(fail);
}

async function modifyArticle(article, success, fail) {
  await axios.put(`/board`, JSON.stringify(article)).then(success).catch(fail);
}

async function deleteArticle(data, success, fail) {
  await axios.delete(`/board`, { data: data }).then(success).catch(fail);
}
```

위 코드처럼 넘겨줄 데이터를 `params`와 `data`라는 이름으로 묶어서 보내주면 서버에서 잘 받을 수 있다.

+

위 방법 말고도 넘겨주는 데이터가 하나인 경우 URI Path에 값을 직접 넣어줘서 해결할 수도 있다.

```javascript
async function getArticle(articleno, success, fail) {
  await axios.get(`/board/${articleno}`).then(success).catch(fail);
}

async function writeArticle(article, success, fail) {
  await axios.post(`/board`, JSON.stringify(article)).then(success).catch(fail);
}

async function modifyArticle(article, success, fail) {
  await axios.put(`/board`, JSON.stringify(article)).then(success).catch(fail);
}

async function deleteArticle(articleno, success, fail) {
  await axios.delete(`/board/${articleno}`).then(success).catch(fail);
}
```


<br>

글 작성을 다하고 나니깐

위에서 RESTful하지 않게 된다는 말에 대한 설명이 필요할 것 같아서 REST API에 대해서도 따로 정리해서 포스팅해야 겠다는 생각이 들었다.

다음 포스팅은 REST API에 관해서 쓸 예정이다.

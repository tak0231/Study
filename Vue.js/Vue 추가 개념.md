
### v-cloack
- Vue.js에서 렌더링 완료 되기 전에 브라우저에서 보이지 않도록 감춰주는 역할을 함
 
- Vue.js의 렌더 flow
  1. 브라우저 페이지 소스 로드
  2. Vue.js 프레임워크 실행
  3. Vue.js 처리
- {{}} 콧수염 표현식 / v-if 등의 조건부 렌더링
  - 아주 잠깐이긴하지만, 그래도 화면에 그대로 노출이 됨
    - 위의 1, 2에서 그대로 노출되고
    - 2->3으로 완전히 넘어가면 비로소 원하는 화면이 나옴
  - 이 노출되는 것을 완전히 방지하는 것이 v-cloack

### ref 속성 (Vue.js)
- 자식 엘리먼트에 접근하는 기능
- 접근해서 데이터를 가져오는 것은 가능 but 엄연히 접근까지의 역할
- getElemnet 함수 / querySelector와 유사한 역할
 - getElemnetById, getElementByClass

```
<div id="app">
 <input type = "text" v-model="value" ref="answer">
</div>
```
- ref 속성 추가

```
this.$ref.answer.focus();
```
- ref 속성 접근 예시
- vue.js / html 모두 사용 가능함
 - vue.js가 사용할 곳에 좌표 찍는 역할이라 보면됨

### 싱글 파일 컴포넌트 체계
- 컴포넌트를 .vue 파일로 구성하는 방식
 - .vue 1개당 컴포넌트 1개

### 뷰 로더
- 웹팩에서 지원하는 라이브러리
- 각 파일에서 ```<template>, <script>,<style>```을 각각 HTML, JS, CSS로 인식되도록 변환해줌
- 
### httpVueLoader
- 웹팩의 빌드 없이 싱글 컴포넌트 파일(.vue)을 사용할 수 있음
- 기존 뷰 로더의 기능 그대로 사용 가능
 - ```<template>, <script>,<style>``` 지원
 - ``` <style scoped> 지원 ```
 - module.export
 - 전통적 CSS / HTML / Scripting Language 모두 사용 가능
 
```
<script>
  var app = new Vue({
    el: '#app',
    data: {
      msg: 'Hello Vue.js!',
    },
    components : {
      'my-component': httpVueLoader('./single.vue'),
    },
  })
</script>
```
- 이런식으로 vue 파일을 컴포넌트로 불러올 수 있음


#### httpVueLoader.register(Vue, URL)
- Vue
 - 뷰 인스턴스
- URL
 - .vue 파일의 경로
- 역할은 httpVueLoader와 동일함
 - 단지 httpVueLoader는 components 속성 안에서 작성되는 것이면
 - httpVueLoader.register는 뷰 인스턴스 밖의 script 영역에서 로드하는 방법


### 모듈
- 관련된 객체들의 

### module.exports

### mixin 속성
![image](https://user-images.githubusercontent.com/80720210/185032626-0bde7193-7c00-4aa1-89b4-d8481d80c043.png)

- 컴포넌트에 무언가 섞어 원하는 거승ㄹ 구현하는 기능
 - 1) Mixin할 객체 만들고
 - 2) 컴포넌트에 객체를 믹스인하고
 - 3) 나머지 부분을 구현함 
- 주의사항
 - 1) HTML/CSS는 믹스인 하지 않음 (순수 자바스크립트로 작성된객체만 가능)
 - 2) 믹스인에서 선언한 속성은 다시 선언할 수 있으며, 이 경우 컴포넌트에 선언된 값을 우선하여 병합함

![image](https://user-images.githubusercontent.com/80720210/185033034-0df69cc3-779b-4c60-bfa3-d649d035c8a7.png)
![image](https://user-images.githubusercontent.com/80720210/185033041-c2ac4cc9-6308-4498-8087-5832945d0342.png)

 - 장점
  - 중복된 코드를 하나의 파일로 캡슐화 가능해짐
  - 오버라이딩 하여 사용도 가능함


### Vue.js data 함수
- Vue 인스턴스에서는 단순히 data : {} 로 사용해도 됨
 - data를 객체로 선언해도 됨
- but Vue 컴포넌트에서는 ```data : functino(){ return {} }``` 의 형태여야 함
 - data를 함수로 선언해야 함
```
<!-- Vue 인스턴스에서 -->
data : {
 message : '안녕하세요'
}

<!-- Vue 컴포넌트에서 -->
data : function(){
 return{
  count : 0
 }
}

```
- 컴포넌트 사용 이유 -> 재사용성 때문
 - 필요할 때마다 인스턴스 생성할 필요 X
 - 꺼내쓰기만 하면 됨

- 컴포넌트에서 data를 함수로 쓰는 이유
 - 재사용 가능한 하위 구성 요소의 개별 인스턴스에 대해 모든 데이터를 포함하는 고유 한 개체가 있는지 확인하기 위함
 - 즉, 이곳 저것에서 컴포넌트 재사용시, 각각의 data를 가지고 있게 할 수 있기 때문
- 컴포넌트에서 만약 ```data : {}```라는 객체로 선언할 경우
 - 하위 구성요소(이 컴포넌트를 사용하고 있는 녀석들)간에 동일한 데이터 개체가 공유되는 문제가 

<br/>

### 참조
- v-cloack : https://swoo1226.tistory.com/172
- ref 속성 : https://ppowerppush.tistory.com/55
- 싱글파일컴포넌트 : / 뷰 로더 : https://owin2828.github.io/devlog/2020/11/06/web-11.html
- http-vue-loader : https://velog.io/@cateto/vue.js-%EC%9B%B9%ED%8C%A9-%EB%B9%8C%EB%93%9C-%EB%B0%A9%EC%8B%9D-%EC%97%86%EC%9D%B4-.vue%EC%8B%B1%EA%B8%80%ED%8C%8C%EC%9D%BC%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%ED%8C%8C%EC%9D%BC%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%95%BC-%ED%95%9C%EB%8B%A4%EB%A9%B4
- MinxIns : https://velog.io/@bluestragglr/Vue.js-Mixin-%EA%B8%B0%EB%8A%A5-%EB%B0%98%EB%B3%B5-%EC%A0%9C%EA%B1%B0%ED%95%98%EA%B8%B0
- vue.js data가 함수여야 하는 경우 : https://heewon26.tistory.com/223
- 뷰 컴포넌트에서 data를 함수로 선언하는 이유 : https://bongra.tistory.com/15

## Vue.js 프로젝트 구조
- 참조 내용 

## 구성 컴포넌트
### TodoHeader
- 애플리케이션 이름 표시
### TodoInput
- 할 일 입력 및 추가
### todoList
- 할 일 목록 표시 및 특정 할일 삭제
### todoFooter
- 할 일 모두 삭제

### 컴포넌트
- 프론트엔드 프레임워크 공통 특징
- 재사용성, 유지 보수 용이하게 해줌

## src/App.vue
```
<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput></TodoInput>
    <TodoList></TodoList>
    <TodoFooter></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue';
import TodoInput from './components/TodoInput.vue';
import TodoFooter from './components/TodoFooter.vue';
import TodoList from './components/TodoList.vue';

export default {
    name: "App",
    components: { 
      TodoHeader, 
      TodoInput, 
      TodoFooter, 
      TodoList 
    }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```
- import
  - 외부의 컴포넌트 가져오는 방법
- Style
  - App.vue 외의 나머지 컴포넌트는 scoped
  - scoped = 해당 컴포넌트에만 스타일을 주기 위함

## src/main.js
```
import Vue from 'vue'
import App from './App'
import { library } from '@fortawesome/fontawesome-svg-core'
import { faCheck, faPlus, faTrash } from '@fortawesome/free-solid-svg-icons';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';


library.add(faPlus, faCheck, faTrash);
Vue.component("font-awesome-icon", FontAwesomeIcon);
Vue.config.productionTip = false;

new Vue({
  render: (h) => h(App)
}).$mount("#app");

```
- main.js
  - 가장 먼저 실행되는 javascript 파일
  - Vue 인스턴스를 생성하는 역할을 함
- import {~} from '@ ~ ~ ~'
  - **경로 별칭**
    - 
- library.add();
- Vue.component();
- FontAwesomeIcon;
- render
<br/>


## src/components/TodoHeader.vue
```
<template>
    <header>
        <h1>TODO LIST</h1>
    </header>
</template>
<style scoped>
h1 {
    color : #2f3b52;
    font-weight : 900;
    margin : 2.5rem 0 1.5rem'
}
</style>
```
- 제목 부분이라 별 상관 없음

## src/components/TodoInput.vue
```
<template>
    <div class="inputBox shadow">
        <input
          type="text"
          v-model="newTodoItem"
          placeholder="이곳에 해야할 일을 적어주세요"
          v-on:keyup.enter="addTodo"
        />
        <span class="addContainer" v-on:click="addTodo">
            <font-awesome-icon class="addBtn" icon="plus" aria-hidden="true"/>
        </span>

        <modal v-if="showModal" @close="showModal = false">
            <h3 slot="header">경고</h3>
            <span slot="body">할 일을 입력해주세요.</span>
            <button slot="footer" @click="showModal = false">닫기</button>
        </modal>
    </div>
</template>
```
- Template 내용
- **@click**
  - 여기서 @는 템플릿 약어



```
<script>
import Modal from "./common/Modal.vue";

export default {
    data(){
        return {
            newTodoItem:"",
            showModal:false
        };
    },
    methods : {
        addTodo(){
            if(this.newTodoItem!=="") {
                let value = this.newTodoItem && this.newTodoItem.trim();
                this.$emit("addTodo",value);
                this.clearInput();
            } else {
                this.showModal = ! this.showModal;
            }
        },
        clearInput(){
            this.newTodoItem="";
        }
    },
    components: {
        Modal : Modal
    }
};
</script>
```
- script 내용

### src/components/common/Modal.vue
- 공식 문서의 Modal 예제를 가져온 것
  - https://vuejs.org/examples/#modal

```
<template>
  <Transition name="modal">
    <div class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">
          <div class="modal-header">
            <slot name="header">default header</slot>
          </div>

          <div class="modal-body">
            <slot name="body">default body</slot>
          </div>

          <div class="modal-footer">
            <slot name="footer">
              default footer
              <button
                class="modal-default-button"
                @click="$emit('close')"
              >OK</button>
            </slot>
          </div>
        </div>
      </div>
    </div>
  </Transition>
</template>

<style scoped>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 300px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 20px 0;
}

.modal-default-button {
  float: right;
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

.modal-enter-from {
  opacity: 0;
}

.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-container,
.modal-leave-to .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
</style>
```
- 변경사항
  - 1) 필요없는 script 태그 내용 전부 삭제
  - 2) template의 div에 있는 v-if 
  - 3) style의 범위만 변경함 (scoped)


## src/components/TodoList.vue
```
<template>
    <section>
        <transition-group name="list" tag="ul">
            <li v-for="(todoItem, index) in propsdata" v-bind:key="todoItem" class="showdow">
                <font-awesome-icon class="checkBtn" icon="check" aria-hidden="true"/>
                {{ todoItem }}
                <span class="removeBtn" type="button" @click="removeTodo(todoItem,index)">
                    <font-awesome-icon icon="trash" ara-hidden="true"/>
                </span>
            </li>
        </transition-group>
    </section>
</template>
```
- Template 내용


```
<script>
export default {
    props : ["propsdata"],
    methods : {
        removeTodo(todoItem, index){
            this.$emit("removeTodo",todoItem,index);
        }
    }
};
</script>
```
- Script 내용


```
<style scoped>
ul{
    list-style-type:none;
    padding-left:0px;
    margin-top:0;
    text-align:left;
}

li {
    display:fles;
    min-height: 50px;
    height:50px;
    line-height:50px;
    margin:0.5rem 0;
    padding:0 0.9rem;
    background : white;
    border-style : solid;
    border-color : #6478fb;
    border-radius:5px;
    vertical-align:middle;
    align-items:center;
    justify-content: center;
}

.checkBtn{
    color:#62acde;
    margin-right:5px;
}

.removeBtn {
    margin-left : auto;
    color : #de4343;
}

.list-enter-active,
.list-leave-active{
    transition:all 1s;
}

.list-enter,
.list-leave-to {
    opacity:0;
    transform:translateY(30px);
}
</style>
```
- style 내용

## src/components/TodoFooter.vue
```
<template>
    <div class="clearAllContainer" @click="clearTodo">
        <span class="clearAllBtn">Clear All</span>
    </div>
</template>
```
- template 

```
<script>
export default {
    methods:{
        clearTodo(){
            this.$emit("removeAll");
        }
    }
};
</script>
```
- script 내용

```
<style scoped>
.clearAllContainer {
    width :8.5rem;
    height:50px;
    line-height:50px;
    background-color:red;
    border-radius:5px;
    margin:0 auto;
    cursor:pointer;
}

.clearAllBtn {
    color : white;
    display : black;
}
</style>
```
- css 내용

## 다시 src/App.vue
```
<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:addTodo="addTodo"></TodoInput>
    <TodoList v-bind:propsdata="todoItems" @removeTodo="removeTodo"></TodoList>
    <TodoFooter v-on:removeAll="clearAll"></TodoFooter>
  </div>
</template>
```
- Template 수정

```
<script>
import TodoHeader from './components/TodoHeader.vue';
import TodoInput from './components/TodoInput.vue';
import TodoFooter from './components/TodoFooter.vue';
import TodoList from './components/TodoList.vue';

export default {
    data(){
      return {
        todoItems:[]
      };
    },
    created(){
      if(localStorage.length > 0){
        for(let i = 0; i<localStorage.length;i++){
          if(localStorage.key(i)!="loglevel:webpack-dev-server"){
            this.todoItems.push(localStorage.key(i));
          }
        }
      }
    },
    methods:{
      addTodo(todoItem){
        localStorage.setItem(todoItem,todoItem);
        this.todoItems.push(todoItem);
      },
      clearAll(){
        localStorage.clear();
        this.todoItems=[];
      },
      removeTodo(todoItem,index){
        localStorage.removeItem(todoItem);
        this.todoItems.splice(index,1);
      }
    },
    components: { 
      TodoHeader, 
      TodoInput, 
      TodoFooter, 
      TodoList 
    }
}
</script>
```
- Script 수정

## Port 변경
### dev port 변경시
- 프로젝트경로/config/index.js
```
module.exports = {
  dev: {

    // Paths
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    proxyTable: {},

    // Various Dev Server settings
    host: 'localhost', // can be overwritten by process.env.HOST
    port: 8077,
```
- 여기의 port 부분을 수정해주면 됨

### vue-cli port 변경시

### 참고
- Vue.js 프로젝트 구조 : https://k39335.tistory.com/64
- To do List 예제 : https://blog.metafor.kr/202
- 템플릿 문법 (약어 포함) : https://kr.vuejs.org/v2/guide/syntax.html
- 

- Port 변경 : https://velog.io/@gillog/Vue-Vue.js-Project-npm-run-dev-Port-%EB%B3%80%EA%B2%BD

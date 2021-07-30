#### why?
sns 프로필페이지의 하단 화면단을 탭 형식으로 구현하면서 템플릿부분과 백엔드에서 정보를 받아오는 소스코드들이  너무 많아서 컴포넌트를 분리하고 싶었다. 단순히 v-if 나 v-show를 쓰면 될 줄 알았는데 게속 안되서 찾아보니 컴포넌트에 탭을 적용하는 방법이 따로 있었다..

### 구조
followPage.vue 라는 상위 컴포넌트에서
followerList.vue , followList.vue 하위 컴포넌트들을 탭 버튼을 통해 렌더링 해준다.

### Code
- template code
```html
<div>
  <button
    class="btn"@click="changeComponent('followerList')">follower
  </button>
    <button
    class="btn" @click="changeComponent('followList')"> follow
  </button>
</div>
<div class="tab-item">
  <keep-alive>
    <component v-bind:is="comp"></component> 
  </keep-alive>
</div>
```
- script code
```javascript
export default {
  components: {
    followList,
    followerList
  },
  data() {
      return { comp: 'followerList' }
  },
  methods: {
      changeComponent: function(componentName) {
          this.comp = componentName
          console.log(this.comp)
      }
  }
}
```
#### 동작 과정
1. 기존에는 팔로워목록이 먼저 보이게 하기 위해 'comp' 변수를 미리 지정해두었다.
2. 버튼을 클릭하면 changeComponent('이름') 함수로 이동
3. 가져온 이름을 'comp' 변수값에 넣는다.
4. v-bind:is를 사용하여 'comp'변수에 해당하는 이름의 컴포넌트로 이동하게 된다.

#### keep-alive
- 컴포넌트 안에서 값을 유지해야 하거나 re-render를 피하기 위해 사용되는 엘리먼트이다.
- keep-alive 태그를 입힌 컴포넌트는 최초로 컴포넌트가 생성되는 시점에 캐시를 해 둔다.
- 이번에 처음 알은 기능,,


##### 팀플하다가 하나 깨달아서 뿌듯하다..

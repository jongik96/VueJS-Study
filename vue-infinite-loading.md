웹sns서비스를 구현하면서 타임라인과 팔로워,팔로잉 목록을 표시할 때
무한스크롤페이징을 구현하려 했다.

아래는 팔로잉 리스트를 불러오는 코드

### 소스코드
template code
```html
<template>
    <div class="followlist">
      <followListImg v-for="(item,index) in following" :key="index" v-bind:name="item.name" v-bind:no="item.no" v-bind:followState="item.followState"/>
      <infinite-loading @infinite="infiniteHandler" spinner="spiral">
        <div slot="no-more" style="color: rgb(102, 102, 102); font-size: 14px; padding: 10px 0px;">더 이상 목록이 없습니다.</div>
      </infinite-loading>
    </div>
</template>
```
script code
```javascript
data:{
  page:0	// 페이지 초기값 지정
}
methods:{
  infiniteHandler($state) {
        axios({
        method: 'get',
        url: `${SERVER_URL}/profile/${userpk}/memfollowings`+"?page=" + (this.page),
        headers: this.getToken,
        }).then(res => {
          setTimeout(() => {  // 로딩스피너를 위해 1초의 지연시간을 설정했다.
            if(res.data.length) {
              this.following = this.following.concat(res.data)
              $state.loaded()
              this.page += 1
                // 끝 지정(No more data) - 데이터가 n개 미만이면 
              if(res.data.length / 20 < 1) {  //종료조건
                $state.complete()
              }
            } else {
                // 끝 지정(No more data)
              $state.complete()
            }
          }, 1000)
        }).catch(err => {
          console.error(err);
        })
      },
}
```

- 위 방법은 vue-infinite-loading 모듈이 설치되어 있어야한다.
- backend에 page의 번호를 함께 전송하면 back에서 해당하는 페이지의 리스트들을 보내주는 방식이다.
	1. 예를 들어 총 22개의 팔로잉 리스트가 있고 20개씩 load 한다면, 첫 요청시에는 0page에 해당하는 20개의 리스트를 받아온 뒤 page를 1 증가시킨다.
	2. 그 다음 1page의 항목들을 요청 한 뒤 해당하는 항목을 추가로 받아온다. 
	3. loop 종료조건을 통해 가져왔던 page의 항목 수를 확인한 뒤 반복하거나 종료한다.
- template코드의 <infinite-loading>은 출력할 div태그의 바로 밑에 위치시켜야 한다.
- infinite-loading 태그의 spinner 속성을 이용해 로딩스피너 모양을 변경할 수 있다.
  
#### fetch문법이 익숙하지 않아 axios로 변경하여 사용했다. fetch문법도 플젝이 끝나고 나면 공부해봐야겠다. ⏳
참고글 : http://yoonbumtae.com/?p=2823


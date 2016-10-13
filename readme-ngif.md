# NgIf Case Study
---

`NgIf`에 집중해보자. 이것은 structural directive의 훌륭한 예시이기도하다. 이것은 boolean값을 취하며, DOM에서의 덩어리 전체를 드러내기도하고, 감추기도 한다.

```html
<p *ngIf="condition">
    condition이 ture이면, ngIf도 ture.
</p>
<p *ngIf="!condition">
    condition이 false이면, ngIf도 false.
</p>
```

`ngIf` directive는 element를 숨기는 것이 아니다. 우리가 브라우저의 개발자도구로 해당 element를 확인해보면, `condition`이 `true`일 경우는 DOM에 존재하지만, `condition`이 `false`일 때에는 DOM element를 단순히 "숨기는" 것이아니라, 없앤다. element가 있을 자리에는 빈 `<script>`태그가 있을 뿐이다. 

> ngIf를 설명할 때, "absent"라는 표현을 사용했는데, 어쩌면 이 표현이 더 이해가 잘 될 수도 있을 것 같다.

![ngIf는 숨기는 것이 아니다.](https://angular.io/resources/images/devguide/structural-directives/element-not-in-dom.png)


---

### 왜 "숨기는" 대신 "지우는" 것일까?
__angular__팀에서는, 특정 단락을 그것의 `display` property를 `none`으로 설정하는 방법으로  "숨기는(hide)"것을 원하지 않는다라고 말했다.

우리가 숨겨놓는다해도 우리 눈에만 보이지 않을 뿐, 해당 요소는 여전히 보이지않은채로 DOM에 남아있다. 여기에 한가지 더 말해두자면, 정확히는 우리는 여기서 `remove`하지 않고 `ngIf`한다.

### 차이는 매우 중요하게 작용한다.
우리가 요소하나를 숨길 때에도, Components는 계속해서 동작하고 있다. 이것은 여전히 남아 이것과 DOM요소에 연결된 그 상태를 유지한다. 또한, 이런 상태로 존재하면 event를 계속 수신하고있는 것이다.  

Angular는 끊임없이 databinding에 영향을 줄 수 있는 변화를 체크하고있다.

비록 보이지는 않지만, Component와 Component의 하위요소들은 더 유용하게 사용할 수 있는 resource들을 묶어버리고있는 셈이다. 사용자에게는 전혀 도움이 되지않는 일들을 하며 성능 및 메모리에 부담을 줄 수 있는 것이다.

### 긍정적인 측면에 대하여
element를 다시 보여주는 것을 굉장히 빠르게 처리할 수 있다. Component의 변경 이전 상태로 되돌리고, 표시할 준비가 되어있다. 이 과정을 진행하는 동안 Component가 다시 초기화 되는 것이 아니다.(다시 초기화하는 것은 너무 부담스러운 작업이다.)

### ngIf는 다르다
말 그대로 `ngIf`는 조금 다르다. 
<br/>

`ngIf`가 `false`로 되어있는경우, component resource 소모에 영향을 미치지 않는다. Angular는 DOM의 element를 제거하고, 관련 Component에 대한 CD(Change Detection)을 중지한다. 또한, 이 것에 붙어있는 DOM Event가 있다면 분리하고, 마지막으로 해당 component를 파괴(destroy)한다.

이를 통해 Component는 가비지컬렉션(garbage-collection)을 할 수 있고, 이상적인 방향인 "최대 사용가능한 메모리 확보"로 향할 수 있게된다.

### Component가 계층구조를 가지고 있을 때의 `ngIf`
많은 경우 Component는 child component를 가지고 있고, 그 child component가 또 child component를 가지고있는 경우도 많다. 이러한 상황에서의 `ngIf`를 생각해보자.

__`ngIf`가 common ancestor를 파괴할 때 그것들 모두가 파괴된다.__
> common : 공통, ancestor : 조상  
이러한 방법으로 깔끔하게 정리해버리는 것은 일반적으로 좋을 작업이라고 보고있다.

<u>언제나 100% 좋다는 말이 아니다. </u>안좋은 경우도 물론 있다. 만약 우리가 component들을 destory 하자마자 특정 component를 필요로하는 경우를 예로 들 수 있겠다. Component를 파괴한 후 다시 구축하는 비용은 많이 들 수 있다. ngIf가 다시 `ture`가 될 때, Angular는 다시 component와 그 subtree를 만든다. 또한 Angular는 모든 component의 초기화 로직을 실행시킨다. Component가 방금 전에 있던 data를 '다시가져오는것'(re-fetches)은 꽤 expensive한 작업이다.

> _Design thought: minimize initialization effort and consider caching state in a companion service._ 
~~초기화 노력을 최소화하고 companion service의 상태를 캐싱하는 것을 고려한다.~~
<br/>

### 결국 뭐가 좋다는건데? - ngIf
"각각의 방법(숨기기와 제거하기)에는 장단점이 있다"라는 것에대해서 나름 세부적으로 살펴봤다. 여기서 angular는 일반적으로 원치않는 Component에 대해서, 단순히 "숨기는(hide)"것 보다는 __`ngIf`를 사용하여 "제거(remove)"하는 것을 권장__하고있다.

```
These same considerations apply to every structural directive, whether built-in or custom.

~~이러한 고려사항으로, 모든 structural directives를 내장(built-in)으로 할지, 사용자정의(Custom) 방식으로 할 지에 대해 적용한다.~~  
```

### 우리는 이에대해 매우 신중하게 생각해야한다.
우리의 application에서의 특정 element를 구성하거나, 파괴하는 일인만큼 그 결과에대해서 ~~당연한 말이지만~~ 신중하게 생각해야한다.

아직 와닿지 않는다면, 이러한 영역으로인해 발생할 수 있는 다이나믹한 상황을 살펴보며 생각해보자.

__예제__
```
For fun, we'll stack the deck against our recommendation and consider a component called heavy-loader that pretends to load a ton of data when initialized.

~~재미를 위해, 우리의 권장사항에 대한 stack the deck, 그리고 Component가 초기화할 때 어마어마한 크기의 데이터를 로드하는 heavy-loader를 호출해본다.~~
```
다음 코드를 실행하면 화면에 component의 두 instances가 표시될 것이다.
```html
<div><!-- Visibility -->
  <button (click)="isVisible = !isVisible">show | hide</button>
  <heavy-loader [style.display]="isVisible ? 'inline' : 'none'" [logs]="logs"></heavy-loader>
</div>
<div><!-- NgIf -->
  <button (click)="condition = !condition">if | !if</button>
  <heavy-loader *ngIf="condition" [logs]="logs"></heavy-loader>
</div>
<h4>heavy-loader log:</h4>
<div *ngFor="let message of logs">{{message}}</div>
```

```typescript
import { Component, Input, OnDestroy, OnInit } from '@angular/core';
let nextId = 1;
@Component({
  selector: 'heavy-loader',
  template: '<span>heavy loader #{{id}} on duty!</span>'
})
export class HeavyLoaderComponent implements OnDestroy, OnInit {
  id = nextId++;
  @Input() logs: string[];
  ngOnInit() {
    // Mock todo: get 10,000 rows of data from the server
    this.log(`heavy-loader ${this.id} initialized,
      loading 10,000 rows of data from the server`);
  }
  ngOnDestroy() {
    // Mock todo: clean-up
    this.log(`heavy-loader ${this.id} destroyed, cleaning up`);
  }
  private log(msg: string) {
    this.logs.push(msg);
    this.tick();
  }
  // Triggers the next round of Angular change detection
  // after one turn of the browser event loop
  // ensuring display of msg added in onDestroy
  private tick() { setTimeout(() => { }, 0); }
}
``` 
두 instances는 toggle에 의해 동작하게 되는데 각 instances는 다음과 같은 특징이 있다.
- 첫 번째 instance : CSS Style에 의해 Visibility가 설정되는 방식
- 두 번째 instance : `ngIf`를 활용하여 DOM에서 빼버리는 방식.

차이점을 조금 더 분명하게 확인하기위해 화면에 log를 표시하여 확인해보기로한다. log는 component가 생성 되거나, 파괴될 때 보이게된다.
- 이 기능은 `ngOnInit`과 `ngOnDestroy` [lifecycle hooks](https://angular.io/docs/ts/latest/guide/lifecycle-hooks.html)를 사용했다.

백문이 불여일견. 결과 이미지를 살펴보자.
![ngIf code test](https://angular.io/resources/images/devguide/structural-directives/heavy-loader-toggle.gif)

두 Component 모두 처음 application을 시작했을 때 DOM에 존재한다. 위 사진의 첫 번째의 경우 부터 살펴보자. 첫 번째 버튼을 토글 했을 때 visibility는 반복적으로, 그리고 빠르게 변경된다. 여기서 Component는 DOM을 벗어나지 않는다. 항상 DOM에 남아있다. 

그다음, `ngIf` 를 활용하는 두 번째 경우를 살펴보자. 여기서 우리는 매번 새로운 instance를 생성하게되며, 그에따라 log가 첫 번째 경우와는 다르게 나타나고있음을 확인할 수 있다. 더하여, 이미지를 자세히보면 첫 번째의 경우보다 이 `ngIf`를 활용하여 heavy하게 처리하고있는 사례에서 약간 버벅이고 있음을 알 수 있다. 

### 상황에 따라 선택하면된다.
만약 정말 빠르고 가볍고, 눈깜짝할 사이에 변경되는 기능을 원한다면 CSS로 Visibility를 컨트롤하는 방법을 사용하는 것이 좋을 수 있다. 그러나 한 가지 더 생각해봐야할 것이 있다.

이제까지 우리가 수 많은 application UI에서, "close"라는 버튼을 클릭했을 때를 떠올려보자. 사실 우리가 "close" 버튼을 클릭하고나서 곧바로 다시 그 버튼의 형태를 보지는 않는다. 아예 다시 그 버튼을 보지않는 경우도 많다. 따라서, 이러한 점까지 고려한다면 `ngIf`를 활용하여 DOM Element 들을 다루는 것이 좋은 방법이라고 볼 수 있다.   


> 이와중에 이 부분을 `ng_play` code에 적용해보려고 했는데, 생각보다 잘 안된다. (16.10.14)

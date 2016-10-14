# Structural Directives
---
이 문서는 Angular의 [Advanced, Structural Directives](https://angular.io/docs/ts/latest/guide/structural-directives.html#!#ngIf)에 대한 내용을 정리한 문서이다.



## Intro
---
__Angular는 우리가 DOM 구조(structure)를 조작할 수 있도록 도와주는 강력한 Template Engine을 가지고 있다.__ 
<br/>
단일페이지 애플리케이션(Single Page Application)을 이루는 기능 중 하나가 바로 __DOM을 조작(Manipulation)하는 기능__이다.
<br/>
사용자가 애플리케이션을 탐색할 때 마다 새로운 페이지를 매번 제공하는 형태가 아니라 DOM의 섹션을 기준으로 표시하고, 애플리케이션의 상태에 따라 보이거나 사라지는 구조로 만드는 것을 추구한다. 
<br/>
이 장에서는 Angular로 어떻게 DOM을 조작하고 다루는지, 그리고 어떻게 이러한 작업들을 우리가 직접 directives를 만들고, 다루면서 해나갈 수 있을지에 대해서 정리해보고자 한다.



<br/><br/>

__기본적으로 이 챕터에는 다음과 같은 내용들이 포함되어있다.__ 
- [learn what structural directives are](https://angular.io/docs/ts/latest/guide/structural-directives.html#definition)
- [study ngIf](https://angular.io/docs/ts/latest/guide/structural-directives.html#ngIf)
- [discover the <template> element](https://angular.io/docs/ts/latest/guide/structural-directives.html#template)
- [understand the asterisk (*) in *ngFor](https://angular.io/docs/ts/latest/guide/structural-directives.html#asterisk)
- [write our own structural directive](https://angular.io/docs/ts/latest/guide/structural-directives.html#unless)


  
<br><br><br>

## NgIf
---
`NgIf`에 집중해보자. 이것은 structural directive의 훌륭한 예시이기도하다. 이것은 boolean값을 취하며, DOM에서의 덩어리 전체를 드러내기도하고, 감추기도 한다.

이 문서에서는 `ngIf`에 대해 다음과 같이 나누어 정리하고있다.
- NgIf api : angular에서 제공하는 기본적인 doc의 내용을 바탕으로 기능 요약, 간단 예제, syntax 등을 길지 않게 정리한다.
- NgIf Case study : 원래 __Structural Directives__ document에서 다루는 내용이다. 'NgIf Api'보다는 조금 더 길고, 세부적인 내용까지 포함한다.



### NgIf Api
---
- [ngIf api](https://angular.io/docs/ts/latest/api/common/index/NgIf-directive.html)
- Class Description : `{expression}`을 바탕으로 DOM Tree의 전체 또는 일부를 삭제하거나 재생성한다.
  
__example__
다음 코드는 `ngIf`를 활용하여 발생하는 error의 수를 세고, 0보다 크면(발생하면) 몇개의 error 가 발생했는지를 표시하도록 하는 작업을 구성하는 코드이다.

 `false`로 평가되는 요소는 DOM 으로부터 제거하고, 그렇지 않은경우(`true`) 요소를 복제(clone)한 요소를 DOM에 다시삽입(reinsert)한다.

```html
<div *ngIf="errorCount > 0" class="error">
  <!-- Error message displayed when the errorCount property on the current context is greater
than 0. -->
  {{errorCount}} errors detected
</div>
```

`ngIf`는 하나지만, 작성하는 방법은 다양하다.
__ngIf Syntax__
```html
<div *ngIf="condition">...</div>
<div template="ngIf condition">...</div>
<template [ngIf]="condition"><div>...</div></template>
```

예제폴더 구성이 어렵다면 Live Example 링크를 통해 동작방법을 살펴보자
- [NgIf Live Sample](http://plnkr.co/edit/fe0kgemFBtmQOY31b4tw?p=preview)


### NgIf Case Study
---
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


  

<br><br><br>

### 왜 "숨기는" 대신 "지우는" 것일까?
---
__angular__팀에서는, 특정 단락을 그것의 `display` property를 `none`으로 설정하는 방법으로  "숨기는(hide)"것을 원하지 않는다라고 말했다.

우리가 숨겨놓는다해도 우리 눈에만 보이지 않을 뿐, 해당 요소는 여전히 보이지않은채로 DOM에 남아있다. 여기에 한가지 더 말해두자면, 정확히는 우리는 여기서 `remove`하지 않고 `ngIf`한다.

<br><br><br>

### 차이는 매우 중요하게 작용한다.
우리가 요소하나를 숨길 때에도, Components는 계속해서 동작하고 있다. 이것은 여전히 남아 이것과 DOM요소에 연결된 그 상태를 유지한다. 또한, 이런 상태로 존재하면 event를 계속 수신하고있는 것이다.  

Angular는 끊임없이 databinding에 영향을 줄 수 있는 변화를 체크하고있다.

비록 보이지는 않지만, Component와 Component의 하위요소들은 더 유용하게 사용할 수 있는 resource들을 묶어버리고있는 셈이다. 사용자에게는 전혀 도움이 되지않는 일들을 하며 성능 및 메모리에 부담을 줄 수 있는 것이다.

<br><br><br>

### 긍정적인 측면에 대하여
element를 다시 보여주는 것을 굉장히 빠르게 처리할 수 있다. Component의 변경 이전 상태로 되돌리고, 표시할 준비가 되어있다. 이 과정을 진행하는 동안 Component가 다시 초기화 되는 것이 아니다.(다시 초기화하는 것은 너무 부담스러운 작업이다.)


<br><br><br>


### ngIf는 다르다
말 그대로 `ngIf`는 조금 다르다. 
<br/>

`ngIf`가 `false`로 되어있는경우, component resource 소모에 영향을 미치지 않는다. Angular는 DOM의 element를 제거하고, 관련 Component에 대한 CD(Change Detection)을 중지한다. 또한, 이 것에 붙어있는 DOM Event가 있다면 분리하고, 마지막으로 해당 component를 파괴(destroy)한다.

이를 통해 Component는 가비지컬렉션(garbage-collection)을 할 수 있고, 이상적인 방향인 "최대 사용가능한 메모리 확보"로 향할 수 있게된다.


<br><br><br>


### Component가 계층구조를 가지고 있을 때의 `ngIf`
많은 경우 Component는 child component를 가지고 있고, 그 child component가 또 child component를 가지고있는 경우도 많다. 이러한 상황에서의 `ngIf`를 생각해보자.


<br><br><br>



__`ngIf`가 common ancestor를 파괴할 때 그것들 모두가 파괴된다.__
> common : 공통, ancestor : 조상  
이러한 방법으로 깔끔하게 정리해버리는 것은 일반적으로 좋을 작업이라고 보고있다.

<u>언제나 100% 좋다는 말이 아니다. </u>안좋은 경우도 물론 있다. 만약 우리가 component들을 destory 하자마자 특정 component를 필요로하는 경우를 예로 들 수 있겠다. Component를 파괴한 후 다시 구축하는 비용은 많이 들 수 있다. ngIf가 다시 `ture`가 될 때, Angular는 다시 component와 그 subtree를 만든다. 또한 Angular는 모든 component의 초기화 로직을 실행시킨다. Component가 방금 전에 있던 data를 '다시가져오는것'(re-fetches)은 꽤 expensive한 작업이다.

> _Design thought: minimize initialization effort and consider caching state in a companion service._ 
~~초기화 노력을 최소화하고 companion service의 상태를 캐싱하는 것을 고려한다.~~
<br/>


<br><br><br>


### 결국 뭐가 좋다는건데? - ngIf
"각각의 방법(숨기기와 제거하기)에는 장단점이 있다"라는 것에대해서 나름 세부적으로 살펴봤다. 여기서 angular는 일반적으로 원치않는 Component에 대해서, 단순히 "숨기는(hide)"것 보다는 __`ngIf`를 사용하여 "제거(remove)"하는 것을 권장__하고있다.

```
These same considerations apply to every structural directive, whether built-in or custom.

~~이러한 고려사항으로, 모든 structural directives를 내장(built-in)으로 할지, 사용자정의(Custom) 방식으로 할 지에 대해 적용한다.~~  
```

<br><br><br>



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
<br/>
두 instances는 toggle에 의해 동작하게 되는데 각 instances는 다음과 같은 특징이 있다.
- 첫 번째 instance : CSS Style에 의해 Visibility가 설정되는 방식
- 두 번째 instance : `ngIf`를 활용하여 DOM에서 빼버리는 방식.

차이점을 조금 더 분명하게 확인하기위해 화면에 log를 표시하여 확인해보기로한다. log는 component가 생성 되거나, 파괴될 때 보이게된다.
- 이 기능은 `ngOnInit`과 `ngOnDestroy` [lifecycle hooks](https://angular.io/docs/ts/latest/guide/lifecycle-hooks.html)를 사용했다.

백문이 불여일견. 결과 이미지를 살펴보자.
![ngIf code test](https://angular.io/resources/images/devguide/structural-directives/heavy-loader-toggle.gif)

두 Component 모두 처음 application을 시작했을 때 DOM에 존재한다. 위 사진의 첫 번째의 경우 부터 살펴보자. 첫 번째 버튼을 토글 했을 때 visibility는 반복적으로, 그리고 빠르게 변경된다. 여기서 Component는 DOM을 벗어나지 않는다. 항상 DOM에 남아있다. 

그다음, `ngIf` 를 활용하는 두 번째 경우를 살펴보자. 여기서 우리는 매번 새로운 instance를 생성하게되며, 그에따라 log가 첫 번째 경우와는 다르게 나타나고있음을 확인할 수 있다. 더하여, 이미지를 자세히보면 첫 번째의 경우보다 이 `ngIf`를 활용하여 heavy하게 처리하고있는 사례에서 약간 버벅이고 있음을 알 수 있다. 


<br><br><br>



### 상황에 따라 선택하면된다.
만약 정말 빠르고 가볍고, 눈깜짝할 사이에 변경되는 기능을 원한다면 CSS로 Visibility를 컨트롤하는 방법을 사용하는 것이 좋을 수 있다. 그러나 한 가지 더 생각해봐야할 것이 있다.

이제까지 우리가 수 많은 application UI에서, "close"라는 버튼을 클릭했을 때를 떠올려보자. 사실 우리가 "close" 버튼을 클릭하고나서 곧바로 다시 그 버튼의 형태를 보지는 않는다. 아예 다시 그 버튼을 보지않는 경우도 많다. 따라서, 이러한 점까지 고려한다면 `ngIf`를 활용하여 DOM Element 들을 다루는 것이 좋은 방법이라고 볼 수 있다.   


> 이와중에 이 부분을 `ng_play` code에 적용해보려고 했는데, 생각보다 잘 안된다. (16.10.14)


---


<br><br><br>

## ngIf 실제로 적용해보기

1. ng_play 디렉토리에 `test`라는 탭을 생성한다.
2. `test`디렉토리에 `ngIf`를 활용한 기본 적인 예제 코드를 작성후 확인한다.

실제로 만들고자 하는 것은 다음과 같다.
![NgIf 활용하기](http://gph.is/2eyEstZ)

__ngIf 실습코드(기본)__

```typescript
//...
@Component({
    selector:'test',
    template:`
        <p> 현재 상태 : {{show}} </p>
        <button (click)="clicked()"></button>
        <div>
            <div *ngIf=show>
                <h2>이 글씨가 나오면 True 라는 이야기지</h2>
            </div>
        </div>
    `
})
export class TestComponent {
    show: boolean = false;
    clicked(){
        this.show = !this.show;
    }
}
```


---
<br/>
<br/>
<br/>

## Discover the `<template>` element
---

`ngIf`와 마찬가지로 `Structure directives`는 [HTML5 template tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/template)를 사용하여 마법을 부린다.

Angular App 외부에서 `<template>`태그의 기본 CSS 표시 속성은 `none`이다. 이것의 내용은 보이지 않는다.

App안에서, Angular는 `<template>`태그와 그 하위 children 요소를 제거한다. contents 는 사라진 것 처럼 보이지만 사라지지않았다.

이러한 효과를 다음 코드와 함께 살펴보면 이해가 쉽다. 앞서 말한 효과로 인해 다음 코드 중 중간에 있는 `hip!`이라는 단락이 `template`로 감싸져(wrapping)있는 것을 볼 수 있다.

```html
<p>
  Hip!
</p>
<template>
  <p>
    Hip!
  </p>
</template>
<p>
  Hooray!
</p>
```
-
```
The display is a 'Hip! Hooray!', short of perfect enthusiasm. The DOM effects are different when Angular is in control.

~~위 코드가 Angular App 외부(Outside of Angular)에 있을때, "Hip! Hooray!"라는 문장이 표시될 것이다. 그리고 Angular app 안에서 컨트롤 될 때, 다른 영향을 받게된다.~~
```

다음 이미지에서 차이점을 확인할 수 있다.
![Outside of Angular and Inside of Angular](https://angular.io/resources/images/devguide/structural-directives/template-in-out-of-a2.png)

오른쪽(Inside of Angular)에서도 보이지만, Angular는 `<template>`태그와 그 안의 콘텐츠를 비어있는 `<script>`태그로 교체(replace)한다.

__이건 단지 기본 동작일 뿐이다.__
이것은 말 그대로 단지 기본 동작을 설명하기 위해 든 예시일 뿐이다. 또한, `ngSwitch`를 활용하여 `<template>` 에 다양한 적용하는것과는 다른 부분이 있다.

```html
<div [ngSwitch]="status">
    <template [ngSwitch]="'in-mission'">In Mission</template>
    <template [ngSwitch]="'ready'">Ready</template>
    <template ngSwitchDefault>Unknown</template>
</div>
``` 

위 코드에있는 여러 `ngSwitch` 구문 중에서 어느 condition 하나라도 `true`이면, Angular는 template의 contents를 DOM에 삽입(insert)한다.

<br/><br/><br/>

### What does this have to do with "ngIf" and "ngFor"?
---
`ngSwitch`도 꽤 쓸만하고, 이미 저것만으로도 꽤 많은 것들을 할 수 있겠다는 생각을 할 것이다. 이제부터는 우리가 __"왜, 그리고 언제 `ngIf`, 그리고 `ngFor`를 사용해야 하는지"__에 대해 생각해볼 필요가 있다.

__우리는 `<template>`태그와 directive들을 사용하지 않았다.__



<br/><br/><br/>

## The asterisk (*) effect
---

`asterisk`는 "별표"라는 뜻이다. 여기에 초점을 맞춰서 생각해보자.

다음 코드를 살펴보자. `NgSwitch`와 다른 차이점이 보이는가?

```html
<div *ngIf="hero">{{hero}}</div>
<div *ngFor="let hero of hereos">{{hero}}</div>
```

`ngIf`와 `ngFor`에는 asterisk, 즉 별표가 접두사로 붙는다.

__The asterisk is "syntactic sugar".__
`ngIf`, `ngFor`코드를 작성할 때 `*`를 사용함으로써, 쓰는사람이나 읽는 사람 모두 편리해진다.

```
Under the hood, Angular replaces the asterisk version with a more verbose `<template>` form.
```


### ngIf를 작성하는 방법들
다음 코드를 살펴보자. `ngIf`를 작성하는 두 가지 방법이 나와있다. 결과는 동일하나 양쪽 어느형태든 우리는 사용할 수 있다.


```html
<!-- Examples (A) and (B) are the same -->
<!-- (A) *ngIf paragraph -->
<p *ngIf="condition">
  Our heroes are true!
</p>

<!-- (B) [ngIf] with template -->
<template [ngIf]="condition">
  <p>
    Our heroes are true!
  </p>
</template>
```
> Angular에서 대부분은 위 코드에서 (A) 형태로 코드를 작성한다고 한다.

```
It's worth knowing that Angular expands style (A) into style (B). It moves the paragraph and its contents inside a <template> tag. It moves the directive up to the <template> tag where it becomes a property binding, surrounded in square brackets. The boolean value of the host component's condition property determines whether the templated content is displayed or not.

정확히 말하면 style (A) 에서 style(B)로 확장가능하기때문에 더 가치가 있다고 보는 것이다. 즉, 위 코드의 `<p>`태그 부분을 `<template>`태그 안에두고 host Component인 `<template>`부분의 `condition` property에 따라 보여질지 안보여질지를 결정하게 둘 수 있다.
```

__Angular transforms `*ngFor` in a similar manner__
Angular는 방금 살펴봤던 `ngIf`와 비슷한 방법으로 `ngFor`에도 이러한 변환을 적용해두었다.


```html
<!-- Examples (A) and (B) are the same -->

<!-- (A) *ngFor div -->
<div *ngFor="let hero of heroes">{{ hero }}</div>

<!-- (B) ngFor with template -->
<template ngFor let-hero [ngForOf]="heroes">
  <div>{{ hero }}</div>
</template>
```

기본 패턴은 동일하다.
1. `<template>` 생성
2. content를 재배치
3. directive를 `<template>`안으로 이동한다.


```
There are extra nuances stemming from Angular's ngFor micro-syntax which expands into an additional ngForOf property binding (the iterable) and the hero template input variable (the current item in each iteration).


```

<br/><br/><br/>

## Make a structural directive
---
여기까지의 내용을 바탕으로 우리의 application에 적용할 structural directive 를 만들어보자.

Let's write our own `structural directive`, an Unless directive, the not-so-evil twin of ngIf.


`ngIf`가 condition이 `true`일 때 표시하는 것과는 달리, 우리의 directive는 condition이 `false`일 때 콘텐츠를 표시한다.


__directive를 만드는 것은 Component를 생성하는 것과 비슷하다.__

- Directives의 Decorator를 가져와 `import` 해준다.
- directive를 식별할 __CSS attribute selector(in brackets)__를 추가한다.
- 바인딩을 위해 input property에 식별가능한 이름을 부여해준다.(일반적으로 directive 자체의 이름)
- decorator를 우리의 implements class에 적용한다.

> implements : 구현하다.

다음과 같이 시작할 수 있다.

```typescript
import { Directive, Input } from '@angular/core';

@Directive({ selector: '[myUnless]' })
export class UnlessDirective {
}
```
> __Selector brackets [ ]__
The CSS syntax for selecting an attribute is a name in square brackets. We surround our directive name in square brackets. See Directive configuration on the [cheatsheet](https://angular.io/docs/ts/latest/guide/cheatsheet.html).

특정 속성(attribute)을 선택하는 CSS Syntax는 대괄호`[]`안에 해당 속성의 이름을 넣는 형태로 작성된다. 이는 기존 CSS Syntax에도 존재하는 것이다.

- [w3schools, css selector attribute](http://www.w3schools.com/cssref/tryit.asp?filename=trycss_sel_attribute)

> __Selector name prefixes__
selector 이름을 정할 때, 접두사를 따서 정하는 것을 권장한다. 또한 selector 이름이 지금 현재는 물론, 미래에도 표준 HTML attribute 이름과 충돌하지 않아야한다는 점을 주지해야한다.



__Our prefix is `my`.__
우리는 Angular 에서 이미 사용하고, 포함되어있는 `ng`라는 접두사를 쓰지 않기로 한다. 이미 어떠한 의미를 가지고 사용되고있는 접두사를 새로운 범위에서 사용할 경우 혼란을 가져올 수 있기 때문이다. 따라서 여기서는 `my`라는 접두사를 사용하기로 한다.


<br/><br/>

__TemplateRef 와  ViewContainerRef__
앞으로 template에 접근하거나, 이것에 대한 콘텐츠 렌더링이 필요할 수 있다. 우리는 `TemplateRef`로 template에 접근할 수 있다. renderer는 `ViewContainerRef`로 한다.

둘다 우리의 __constructor__에 `private variable` 형태로 주입(Inject)하자. 

_constructor에 `TemplateRef`와 `ViewContainerRef` Inject 하는 코드_

```typescript
constructor(
    private templateRef: TemplateRef<any>,
    private viewContainerRef: ViewContainerRef
) { }
```

- 우리 directive의 `Consumer`는 `boolean` value를 우리 directive의 `Unless` input property에 바인딩 할 것이다.
    - directive는 이 값을 기준으로 template를 추가하거나 제거한다.

<br/><br/>

__이제 stter-only property인 `myUnless`property를 추가해보자.__

```typescript
@Input() set myUnless(condition: boolean){
    if(!condition){
        this.viewContainer.createEmbeddedView(this.templateRef);
    } else {
        this.viewContainer.clear();
    }
}
```

> 여기서 `@Input()`은  이 속성은 directive를 위한 input property임을 알려주는 annotation이다.

우선 여기까지는 신기할게 전혀 없다. 만약 condition이 `false`면 우리는 template를 렌더링하고, 다른 경우라면 element content를 `clear()`시킨다.

최종 결과는 다음과 같아야 한다.

~~`unless.directive.ts`~~

```typescript
import { Directive, Input } from '@angular/core';
import { TemplateRef, ViewContainerRef } from '@angular/core';
@Directive({ selector: '[myUnless]' })
export class UnlessDirective {
  constructor(
    private templateRef: TemplateRef<any>,
    private viewContainer: ViewContainerRef
    ) { }
  @Input() set myUnless(condition: boolean) {
    if (!condition) {
      this.viewContainer.createEmbeddedView(this.templateRef);
    } else {
      this.viewContainer.clear();
    }
  }
}
```

이제 이것을 __AppModule__의 `declarations` array에 추가한다. 일단 처음에는 임시로 template에서 쓸 test HTML 코드를 작성해둔다.

```html
<p *myUnless="condition">
    condition is false and myUnless is ture.
</p>
<p *myUnless="!condition">
    condition is true and myUnless is false.
</p>
```

작성 후 이것을 실행하여 여러 동작들을 체크해가며 확인해보도록 하자.
- 이것은  (우리가 하고자 했던대로) `ngIf`와 반대로 작동하고있다.


condition 값이 `true`이면 상단의 단락을 제거한다(정확히는 `<script>` 태그로 replace하는 것이다)그리고 아래부분에서 단락이 나타도록 한다. 여기까지 내용은 아래의 이미지와 같다.

![ng2 make a structural directive](https://angular.io/resources/images/devguide/structural-directives/myUnless-is-true.png)

이미지를 통해 확인해보면 우리의 `Unless` directive는 확실히 간단하게 죽는(dead)다. 

__ngIf는 분명 더 복잡할 것이다?__
이런 궁금증이 (만약에라도 생긴다면) 이제까지 작성한 소스코드만 보더라도 아니라는 생각이 들것이다. 소스코드는 꽤 잘 문서화 되어있기때문에 우리가 이중에서 뭔가가 작동하는 방법을 알고 싶을 때는 소스에대한 자문을 구하는 것에 부끄럼없이 접근할 수 있다.


__`ngIf`는 크게 다르지 않다.__
크게다르지 않다. 단지 성능 향상 및 개선을 위해 몇가지 체크 사항만 추가되었을 뿐이다. (미처 제거되지 않거나 필요하지 않은 view를 체크) 그 외의 부분들은 거의 동일하다.



<br/><br/><br/>

## Wrap up
---
다음은 이 장에서 다뤘던 소스이다.

~~ 소스영역 ~~

이번 장에서 HTML layout을 `ngFor`나 `ngIf`와 같은 __structural directive__를 활용하여 다루는 것에 대해서 정리해봤다. 그리고 더하여 직접 `myUnless`라는 이름의 __structural directive__를 작성하기도 했다. 

Angular는 이렇게 layout을 지혜롭게 다루는 여러 기술을 제공하고 있다. 특히 structural directive와 같이 외부 콘텐츠를 가지고 자신의 template안에서 해당 콘텐츠를 통합해서 사용할 수 있는 component 등을 제공하고 있다. 
- tab과 tab pane을 컨트롤하는 기능이 좋은 예라고 할 수 있다.?


<br/><br/><br/>


## (다음 장에서는) __Structural Component__ 에 대해서 살펴볼 계획이다.
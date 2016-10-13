

# Readme
---



# Angular2 Material
---
- [Angular2 Material 공식페이지](https://justindujardin.github.io/ng2-material/#/)

## Components
---
Angular2 Material에서 제공하는 Component 목록

- [Button](https://justindujardin.github.io/ng2-material/#/components/button)
- [Card](https://justindujardin.github.io/ng2-material/#/components/card)
- [Checkbox](https://justindujardin.github.io/ng2-material/#/components/checkbox)
- [Data Table](https://justindujardin.github.io/ng2-material/#/components/data-table)
- [Dialog](https://justindujardin.github.io/ng2-material/#/components/dialog)
- [Elevation](https://justindujardin.github.io/ng2-material/#/components/elevation)
- [Input](https://justindujardin.github.io/ng2-material/#/components/input)
- [List](https://justindujardin.github.io/ng2-material/#/components/list)
- [Pagination](https://justindujardin.github.io/ng2-material/#/components/pagination)
- [Progress Bar](https://justindujardin.github.io/ng2-material/#/components/progress-bar)
- [Progress Circle](https://justindujardin.github.io/ng2-material/#/components/progress-circle)
- [Radio](https://justindujardin.github.io/ng2-material/#/components/radio)
- [Sidenav](https://justindujardin.github.io/ng2-material/#/components/sidenav)
- [Switch](https://justindujardin.github.io/ng2-material/#/components/switch)
- [Tabs](https://justindujardin.github.io/ng2-material/#/components/tabs)
- [Toolbar](https://justindujardin.github.io/ng2-material/#/components/toolbar)


## install
---
### Installing from NPM

Install the `ng2-material` and the material2 core libraries:
```
npm install --save ng2-material @angular2-material/core
```

그런다음, material directives와 providers를 `import`한다.
```
import {MATERIAL_DIRECTIVES, MATERIAL_PROVIDERS} from "ng2-material";
```


Then reference the styles in your page
```
<link rel="stylesheet" href="node_modules/ng2-material/ng2-material.css">
<link rel="stylesheet" href="node_modules/ng2-material/font/font.css">
```
---






## 오늘 배울 것
---
[material2-app](https://material2-app.firebaseapp.com/)에서 오늘 배울 'Angular2 CLI에서 material 사용하기'라는 주제의 결과화면을 확인할 수 있다.



__create structure for us__
```
ng new ng2_play
```

[ng2play live demo](https://ng2play.azurewebsites.net/)


## Getting Started
---

`angualr-cli`와 `typings`를 글로벌(-g)로 먼저 설치한다.
```
npm -g install angular-cli
npm -g install typings
```

To run the frontend
```

// To run the frontend
git clone https://github.com/ajtowf/ng2_play.git
cd ng2_play
npm install
ng serve
```
여기까지 작성하면, 다음과 같은 형태의 웹페이지가 구성된다.
![ng2play ngserve 실행화면]()


// To run the backend
```
cd ng2_play\backend
node app.js 
```


## API
---


### _Angular 2 Material grid list__

- 요약 : `md-grid-list`는 cell을 grid-based layout에서 배열하는 alternative list view이다.
  - [Material Design spec 확인하기](https://www.google.com/design/spec/components/grid-lists.html)    
#### 사용법

__Simple grid list__
`md-grid-list`를 사용하기 위해서는 `MdGridList` module을 우리의 application의 `NgModule`에 `import`해주어야 한다.


_my-app-module.ts_
```typescript
import {MdGridListModule} from '@angular2-material/gridlist/gridlist';
 
@NgModule({
  imports: [MdGridListModule],
  ...
})
export class MyAppModule {}
```
우리의 template 안에서, `md-grid-list`요소를 생성하고, 우리가 몇 개의 열(colmn)로 이루어진 그리드를 만들 것인지에 맞춰서 `cols`property를 특정해주어야한다. (이것이 유일하게 의무적으로 적어주어야할 부분이다.)

우리는 각각의 타일을 `md-grid-tile` 요소를 사용하여 정의한다. 모든 tile content는 이 사이에서 전달되게 된다.

__그리드에 몇개의 행이 만들어질까?__
Tile은 그리드의 첫 번째 위치에 배치되기때문에, 행(row)의 숫자는 얼마나 많은 수의 Tile을 각 row에 맞춰 넣을 수 있는지, 그리고 몇개의 열(column)있는지, 그리고 각 Tile의 `colspan`과 `rowspan`에 의해 결정된다.

다음 코드를 보면서 생각해보자.

```
<md-grid-list cols="4" [style.background]="'lightblue'">
   <md-grid-tile>One</md-grid-tile>
   <md-grid-tile>Two</md-grid-tile>
   <md-grid-tile>Three</md-grid-tile>
   <md-grid-tile>Four</md-grid-tile>
</md-grid-list>
```
위 코드는 다음과 같은 형태로 나타나게된다.
![npm @angular2-material/grid-list](https://material.angularjs.org/material2_assets/grid-list/basic-grid-list.png)

__Grid list config__
- `cols`
  - `cols` property는 우리의 grid에 몇개의 column이 보여질지를 컨트롤한다. 이것은 필수적으로 설정되어야 하며, grid list로는 우리의 layout을 생성(generate)할 수 없다는 것을 기억하자.
  - `cols` 사용 예시 : `<md-grid-list cols="3">`
  - `Default` : 특별히 이유가 있어서 Default 값을 정해놓은 것은 아니다.단, 이 값이 설정되어있지 않을 경우 `md-grid-list`는 에러메시지를 반환할 것이다.
- `rowHeight`
  - list에서 행의 높이는 세 가지 방법으로 측정할 수 있다.
    1. `Fixed height` : height는 (일반적인 CSS 개념과 동일하게) `px`, `em`또는 `rem`으로 설정된다. 만약 단위가 명시되어있지 않으면, `px`로 추정하여 처리하게된다.
      - `Fixed height` 예시 : `<md-grid-list cols="3" rowHeight="100px">...`
    2. `Ratio` : 비율(Ratio)는 width:height 이다. 따라서 반드시 콜론(:)을 포함하여 작성한다.
      - `Ratio` 예시 : `<md-grid-list cols="3" ratio="4:3">`
    3. `Fit` : 이 방법은 height 값을 자동으로 행(row)의 갯수로 나누어 적용하는 방법이다. 주의할 것은 이 방법을 사용할 때 반드시 `grid-list`의 height를 설정하거나 또는 이것이 포함되는 container가 설정되어야한다는 것이다.
      - `Fit` 예시 : `<md-grid-list cols="3" rowHeight="fit">`
  - Default : 기본으로 설정되는 행의 높이는 너비와 높이 기준 1:1비율이다.(1:1 ratio of width:height)
- `gutterSize`
  - Gutter의 size는 `gutterSize` property를 `px`, `em`, `rem` 값을 이용하여 설정할 수 있다.
    - `Gutter`의 사전적 정의는 '도랑'이다. 따라서 각 cell 사이에 물이흐르는 '도랑'이 있다고 생각하면 이해가 쉽다.
    - `gutterSize` 예시 : `<md-grid-list cols="3" gutterSize="4px">...`
    - default size는 `1px`이다.

__Grid tile config__
위에서 Grid list에 대한 configuration을 했다면, 여기서는 __tile__에 대한 configuration을 살펴본다. ~~다행스럽게도 grid list config보다 훨씬짧다.~~

- 우리는 `rowspan`과 `colspan` property를 활용하여 을 각각을 독립적으로 설정할 수 있다.
- Default : `rowspan`과 `colspan`모두 설정되지 않았을 경우, 기본값은 `1`이다.

```typescript
<md-grid-list cols="4" rowHeight="100px">
  <md-grid-tile *ngFor="let tile of tiles" [colspan]="tile.cols" [rowspan]="tile.rows" [style.background]="tile.color">
    {{tile.text}}
  </md-grid-tile>
</md-grid-list>
``` 

```typescript
//...
export class MyComponent {
  tiles: any[] = [
    {text: 'one', cols: 3, rows:1, color: 'lightblue'},
    {text: 'two', cols: 1, rows:2, color: 'lightgreen'},
    {text: 'three', cols: 1, rows:1, color: 'lightpink'},
    {text: 'four', cols: 2, rows:1, color: '#DDBDF1'},
  ];
}
```
위 코드의 결과 이미지는 다음과 같다.
![Angualr2 Material grid-list output image](https://material.angularjs.org/material2_assets/grid-list/fancy-grid-list.png)




- __Angular 2 Material sidenav__
    - 요약 : `MdSidenav`는 Material 2의 side navigation component이다. 
    - 구성 : `MdSidenav`에는 두 가지 Component가 포함되어 있다.
        - `<md-sidenav-layout>` : parent component. 1~2개의 `sidenav` code를 필요로한다.
            - property
                - start
                - end
        - `<md-sidenav>` : sidenav pannel. 
            - property
                - align : start, end
                - mode : over, push, side
                - opened : boolean
            - events
                - open-start
                - close-start
                - open
                - close
            - method
                - open():Promise<void>
                - close():Promise<void>
                - toggle(): Promise<void>
            - note
                - `<md-sidenav>`는 그 안에 포함되는 Content의 사이즈에 따라 resize된다.
                - `<md-sidenav>`적용시 CSS에 다음과 같은 코드를 추가해준다.
                    - `md-sidenav { width: 200px; }`
                - 아직 resize event를 지원하지는 않기때문에, percent(%)기반의 사이즈 조정기능은 피하는 것을 권장한다.

- __ 예제 : Angular 2 Material sidenav__
```html
<app>
  <md-sidenav-layout>
    <md-sidenav #start (open)="mybutton.focus()">
      Start Sidenav.
      <br>
      <button md-button #mybutton (click)="start.close()">Close</button>
    </md-sidenav>
    <md-sidenav #end align="end">
      End Sidenav.
      <button md-button (click)="end.close()">Close</button>
    </md-sidenav>
 
    My regular content. This will be moved into the proper DOM at runtime.
  </md-sidenav-layout>
</app>
```

- [@angular2-material/sidenav](https://www.npmjs.com/package/@angular2-material/sidenav)



- __@Angular 2 Material toolbar__
    - ![@Angular 2 Material toolbar image](https://cloud.githubusercontent.com/assets/4987015/13727769/6d952c78-e900-11e5-890a-ccfd46996812.png)
    - 요약 : `MdToolbar`는 수직 정렬된 바(vertical aligned bar)이다. 이 `MdToolbar`에 application의 title 또는 특정 action을 넣을 수 있다.
    - 구성 : `<md-toolbar>`
    - 속성(property) :
        - color : primary, accent, warn
    - 정렬(Alignment)
        - `<md-toolbar-row>`안의 요소들을 정렬하는 것은 어렵지 않게 `flexbox`layout을 사용하여 할 수 있다.
    - Notes : `md-toolbar` component는 기본적으로 background palette를 사용한다. 
    

- __ 예제1 : Angular 2 Material toolbar__기본적인 예제 
```html
<md-toolbar [color]="myColor">
    <span> My Application Tiltle</span>
</md-toolbar>
```

- __ 예제2: Angular 2 Material toolbar__ multiple rows 예제
`md-toolbar`를 여러줄로 작성할 수 있다. multiple row는 `<md-toolbar>` 요소 안에  `<md-toolbar-row>` 태그를 넣어주면 만들 수 있다.



```html
<md-toolbar [color]="myColor">
  <span>First Row</span>
  
  <md-toolbar-row>
    <span>Second Row</span>
  </md-toolbar-row>
  
  <md-toolbar-row>
    <span>Third Row</span>
  </md-toolbar-row>
</md-toolbar>
```


- __ 예제3: Angular 2 Material toolbar__ multiple rows 예제
`md-toolbar`를 여러줄로 작성할 수 있다. multiple row는 `<md-toolbar>` 요소 안에  `<md-toolbar-row>` 태그를 넣어주면 만들 수 있다.


- [@angular2-material/toolbar](https://www.google.co.kr/url?sa=t&rct=j&q=&esrc=s&source=web&cd=3&ved=0ahUKEwjHs9b8jdbPAhWJs48KHeibCUYQFggqMAI&url=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2F%40angular2-material%2Ftoolbar&usg=AFQjCNHLhtCxqa-q_t4NjMZ0Xyd8TJqffA&sig2=jmsw86lm-42sj5y60-6cqQ&bvm=bv.135475266,d.c2I)






- __ 예제4: Angular 2 Material toolbar__ Alignment
    - `<md-toolbar-row>`안의 요소들을 정렬하는 것은 어렵지 않게 `flexbox`layout을 사용하여 할 수 있다.

다음 코드에서는 `<span>Right Aligned Text</span>` 요소가 화면에서 우측정렬되어 나타나도록 하는 코드이다.
```html
<md-toolbar color="primary">
  <span>Application Title</span>
  
  <!-- This fills the remaining space of the current row -->
  <span class="example-fill-remaining-space"></span>
  
  <span>Right Aligned Text</span>
</md-toolbar>
```

```css
.example-fill-remaining-space {
  // This fills the remaining space, by using flexbox.  
  // Every toolbar row uses a flexbox row layout. 
  flex: 1 1 auto;
}
```
## 코드 내용 탐색
---


```html
<md-sidenav-layout [class.m2app-dark]="isDarkTheme"></md-sidenav-layout>
```
전체 code를 감싸는 Container역할을 하는 태그이다. 위에서도 정리했지만, 링크를 찾기 귀찮다면, [@angular2-material/sidenav](https://www.npmjs.com/package/@angular2-material/sidenav)를 통해 이 로직을 확인할 필요가 있다. 분명한 건, 아래나오는 코드는 몽땅 여기 안으로 들어간다는 것이다.



```html
<md-sidenav #sidenav mode="side" class="app-sidenav"> 사이드바 헤더 </md-sidenav>
```
이 예제의 container 이름에서도 느꼈겠지만, sidenavigation이라는 이름답게 사이드바 헤더를 할 수 있도록 여러가지 필요사항을 점검해보고자 했다. 


```html
<md-sidenav-layout [class.m2app-dark]="isDarkTheme">

  <md-sidenav #sidenav mode="side" class="app-sidenav"> 사이드바 헤더 </md-sidenav>

  <md-toolbar color="primary">
    <button class="app-icon-button" (click)="sidenav.toggle()">
      <i class="material-icons app-toolbar-menu">menu</i>
    </button> 해양사고시각화 &nbsp;

    <button md-button [routerLink]="['/']">Home</button>
    <button md-button [routerLink]="['/about', 'Hello, world!']">About</button>

    <span class="app-toolbar-filler"></span> <!-- ? -->

    <button md-button [routerLink]="['/material']">Material Demo</button>
    <!-- login element view (by ngIf) -->
    <button md-button *ngIf="loggedIn()" [routerLink]="['/profile']">Profile</button>
    <button md-button *ngIf="!loggedIn()" (click)="login()">Sign in</button>
    <button md-button *ngIf="loggedIn()" (click)="logout()">Sign out</button>
    
    <!-- Click event (with boolean value) -->
    <button md-button (click)="isDarkTheme = !isDarkTheme">TOGGLE THEME</button>
  </md-toolbar>
  

  <!-- ? -->
  <div class="app-content">
    <router-outlet></router-outlet>
  </div>
  

</md-sidenav-layout>
<!-- 우측하단 체크버튼 -->
<span class="app-action" [class.m2app-dark]="isDarkTheme">
  <button md-fab><md-icon>check circle</md-icon></button>
</span>

```




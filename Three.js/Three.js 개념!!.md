Three.js 는 웹 페이지에 3D 객체를 손쉽게 렌더링 할 수 있도록 도와주는 JS 3D 라이브러리 입니다.
이 라이브러리는 WebGL 기술을 바탕으로 `scene`, `camera`, `renderer` 기술을 간단하게 사용할 수 있습니다.
그렇지만 배우고자 하는 입문자들에게는 난이도가 있는 기술이라서 충분한 학습 기간을 가지고 배우는것을 추천합니다!!!

__WebGL이란__
* [WebGL](https://www.khronos.org/webgl/)은 플러그인을 사용하지 않고 [OpenGL ES](https://www.khronos.org/opengles/) 2.0 기반 API를 이용하여 브라우저의 HTML [`canvas`](https://developer.mozilla.org/en-US/HTML/Canvas)에 렌더링하여 3D 웹 콘텐츠 제작을 가능하게 합니다.
* WebGL 만을 사용하여 코드를 작성하게 된다면 복잡한 코드를 작성하게 될 것 입니다. 
    Three.js 는 이런 3D 요소의 처리를 도와 보다 직관적인 코드 작성하게 도와줍니다.


### 설치
Three.js 는 CDN 을 이용하거나 npm 으로 설치를 진행하여서 하는데 
three.js document를 가보시면 option1. install with npm and build tool 이라는걸 볼 수 있습니다. 


npm에 three module을 추가하고 vite 전역 설치하고 vite를 실행하면 잘 동작할 것입니다.
```bash
npm install --save three 
npm install --save-dev vite
npx vite
```


## Three.js 시작!!
### Scene 생성
Three.js 는 크게 `scene`, `camera`, `renderer` 로 나뉩니다.

그 중 Scene은 렌더링할 모든 객체와 광원을 저장하는 공간입니다.
`Scene`이 없으면 어떠한 객체도 표시할 수 없기 때문에 필수적으로 생성해야 합니다.

아래 예제 코드 주석을 살펴봅시다!!


```javascript
import * as THREE from "three"

// `three` 모듈의 `Scene()`메서드를 활용하여 새로운 `Scene` 객체를 생성해봅시다.
const scene = new THREE.Scene()
scene.background = new THREE.Color("skyblue");  // Scene의 배경색은 skyblue로!!


const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
)

const renderer = new THREE.WebGLRenderer()
renderer.setSize(window.innerWidth, window.innerHeight)
document.body.appendChild(renderer.domElement)

```

### Camera 생성
`Camera`는 `Scene` 객체를 어떻게 촬영하여 보여줄 것인가를 결정합니다.
카메라에는 원근법을 무시하는 `OrthographicCamera` 와 
원근법을 적용하여 일상생활에서 우리가 보는 것처럼 장면을 보여주는 `PerspectiveCamera`
가 있습니다.
```javascript
const camera = new THREE.PerspectiveCamera(
  75, // field of view
  window.innerWidth / window.innerHeight, // aspect
  0.1, // near
  1000 // far
)

// 카메라의 위치 설정 (x: 0, y: 0, z: 5)
camera.position.set(0, 0, 5);

```
매개변수는 아래와 같습니다.
1. `field of view` : 시야각으로, 해당 시점의 화면이 보일 정도를 의미.
2. `aspect` : 비율을 의미한다. 우리는 웹 브라우저를 통해 사물을 바라보기 때문에 
   보통화면의 width를 height로 나눈 값을 전달합니다.
3. `near` : 해당 수치보다 가까이 있는 물체는 보이지 않습니다.
4. `far` : 해당 수치보다 멀리 있는 물체는 보이지 않습니다.

`near` 과 `far`는 프로그램의 성능 향상을 위해 사용할 수 있습니다.


### 3D 모델 불러오기
Three.js 에서는 `Mesh` 생성자를 활용하여 `Scene`에 객체를 추가해줄 수 있다.
`Mesh` 생성자는 매개변수로 `geometry` 와 `meterial` 정보를 전달받습니다.

예제에서는 Loader 객체를 통해 3D 모델을 사용해봅시다.

`GLTFLoader` 객체를 import 합니다.
해당 객체는 `loader` 변수에 저장하고, 내장 메서드인 `load()` 를 활용하여 다운로드한 3D 모델을 불러옵니다.
불러 오는데 성공하였다면 `Scene`에 추가 할 수 있는 콜백함수를 활용하여 `scene.add()` 메서드를 활용합니다.
[웹 3D 모델 페이지](https://sketchfab.com/Sketchfab/models)
```javascript
import { GLTFLoader } from "three/addons/loaders/GLTFLoader.js";

let loader = new GLTFLoader();

loader.load("sketchfab/scene.gltf", function (gltf) {
  scene.add(gltf.scene);
});

```

### Light 추가!!
우리는 물체를 보기 위해 빛이 있어야 하는 것처럼, Three.js 에서도 물체를 보기 위해서 조명을 추가합니다.
`Three.js` 에서는 다양한 종류의 조명을 지원합니다.

#### AmbientLight

`AmbientLight`는 광원, 즉 빛의 시작점이 없이, 물체의 모든 면을 골고루 비춰주는 빛입니다. 
매개 변수로 빛의 색상인 `color`와 빛의 강도를 의미하는 `intensity`를 받는다. 선언한 뒤 색상과 강도를 변경하고 싶을 경우 아래와 같이 사용할 수 있습니다.

```javascript
let light = new THREE.AmbientLight();

light.color = 0xffff00;
light.intensity = 5;
```

#### HemisphereLight

하늘과 바닥 두 곳의 광원을 가지는 빛입니다.
```js
let light = new THREE.HemisphereLight(0xffff00, 0xff0000, 0.3);
```


#### DirectionalLight

**태양**과 같이 무한대의 먼 거리에서 모든 물체에 같은 각도로 비추는 빛입니다.
```js
let light = new THREE.DirectionalLight(0xffff00, 0.7);
```

#### PointLight

**전구**처럼 한 지점에서 모든 방향으로 방출하는 빛을 제공합니다. 매개변수로 `distance`를 넘겨 받아 빛이 방출되는 거리를 지정할 수 있고, `decay`를 통해 빛이 거리에 따라 얼마나 어두워지는지를 결정할 수 있다. 기본값은 각각 0과 1입니다.

```js
let light = new THREE.PointLight(0xffffff, 0.5, 15, 3);
```


### 렌더링 Renderer!
`Renderer` 는 `Three.js` 의 핵심 객체라고 할 수 있습니다.
`Scene` 과 `Camera` 객체를 넘겨 받아 랜더링 합니다.

먼저 `WebGLRenderer`를 통해 `renderer` 객체를 생성해줍니다. 
이 때, 생성자 함수는 랜더링한 것들을 보여줄 `Canvas` 태그를 매개변수로 받습니다.
그리고 3D 모델의 테두리를 부드럽게 하기 위해 `antialias: true`를 추가해줍니다.

```js
let renderer = new THREE.WebGLRenderer({
  canvas: document.querySelector("#canvas"),
  antialias: true,
});

renderer.outputEncoding = THREE.sRGBEncoding;
renderer.setSize(window.innerWidth, window.innerHeight);
```

보다 정확한 색상을 랜더링하기 위해 `outputEncoding` 값을 설정해주었고, `setSize` 메서드를 활용하여 `renderer`의 크기를 조절해줍니다.

이제 `loader` 객체의 콜백 함수 내에서 `renderer`의 `render` 메서드를 통해 랜더링해줍니다.

```js
loader.load("duck/scene.gltf", function (gltf) {
  scene.add(gltf.scene);
  renderer.render(scene, camera); // 랜더링
});
```

### 느낀점
Three.js 를 이용하여 3D 모델을 불러오는것만으로도 이렇게 힘들줄 몰랐습니다.

이러한 3D 모델을 이벤트 핸들링을 하여 인터랙션 하게 된다면 어떠한 웹이 나올지 무궁무진하다는 것을 느꼈습니다. 

Three.js를 학습하는데 생각보다 더 오래 걸리겠구나 라는 생각이 들었습니다....

```js
import * as THREE from "three"
import { GLTFLoader } from "three/addons/loaders/GLTFLoader.js"

// `three` 모듈의 `Scene()`메서드를 활용하여 새로운 `Scene` 객체를 생성해봅시다.
const scene = new THREE.Scene()
scene.background = new THREE.Color("skyblue")
// Camera
const camera = new THREE.PerspectiveCamera(
  45,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
)

camera.position.set(0, 0, 5) // 카메라 위치선정

let loader = new GLTFLoader()
loader.load("./sketchfab/scene.gltf", function (gltf) {
  scene.add(gltf.scene)
  renderer.render(scene, camera) // 랜더링
})

let renderer = new THREE.WebGLRenderer({
  canvas: document.querySelector("#canvas"),
  antialias: true,
})

renderer.outputColorSpace = THREE.SRGBColorSpace
renderer.setSize(window.innerWidth, window.innerHeight)

document.body.appendChild(renderer.domElement)
```
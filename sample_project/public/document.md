# Benjamin Pedley B00823900 Element 5 description

## Scene One

I first made a skybox through the methods and assets we had learnt before: 
 ``` typescript

function createSky(scene: Scene) {
  const skybox = MeshBuilder.CreateBox("skyBox", { size: 150 }, scene);
  const skyboxMaterial = new StandardMaterial("skyBox", scene);
  skyboxMaterial.backFaceCulling = false;
  skyboxMaterial.reflectionTexture = new CubeTexture(
    "./assets/textures/skybox/skybox",
    scene
  );
  skyboxMaterial.reflectionTexture.coordinatesMode =
    Texture.SKYBOX_MODE;
  skyboxMaterial.diffuseColor = new Color3(0, 0, 0);
  skyboxMaterial.specularColor = new Color3(0, 0, 0);
  skybox.material = skyboxMaterial;
  return skybox;
} 
```

I then created the ground and the box in different colours, along with a light and camera.

With the box, I wanted to implement movement, and I did this through the following code: 

``` typescript
const inputMap: { [key: string]: boolean } = {};
``` 
This tracks key presses from the player. 

``` typescript 
  that.scene.actionManager = new ActionManager(that.scene);
  that.scene.actionManager.registerAction(new ExecuteCodeAction(ActionManager.OnKeyDownTrigger, (evt) => {
    inputMap[evt.sourceEvent.key] = true;
  }));
  that.scene.actionManager.registerAction(new ExecuteCodeAction(ActionManager.OnKeyUpTrigger, (evt) => {
    inputMap[evt.sourceEvent.key] = false;
  }));
  ``` 
  This tells the action manager whether a key is being pressed or not. 

``` typescript 
  that.scene.onBeforeRenderObservable.add(() => {
    if (that.box) {
      if (inputMap["w"]) {
        that.box.position.z -= 0.1;
      }
      if (inputMap["s"]) {
        that.box.position.z += 0.1;
      }
      if (inputMap["a"]) {
        that.box.position.x -= 0.1;
      }
      if (inputMap["d"]) {
        that.box.position.x += 0.1;
      }
    }
  });

  return that;

  ```
If WASD keys are pressed, the box's positions will change by 0.1, and the scene will update to reflect this.


  ## Scene Two

For Scene Two, I went for a more creative approach. My idea was to create a strange monkey club where the monkeys watch you.

I first made the ground, two spheres, basic lighting, and a camera.
I applied textures of monkeys' faces stretched across these spheres to make them appear more odd.
I made the spheres follow the camera using this code:

```typescript 
 scene.registerBeforeRender(() => {
    const camera = scene.activeCamera;
    if (camera) {
      sphere.lookAt(camera.position);
    }
  });
  ```
This works by checking the camera's position before every frame and then updating the spheres' positions accordingly.

I also made flashing spotlights using this code:

```typescript 
unction animateLights(scene: Scene, light1: SpotLight, light2: SpotLight) {
  let light1Increasing = true;
  let light2Increasing = false;

  scene.onBeforeRenderObservable.add(() => {
    if (light1Increasing) {
      light1.intensity += 0.1; 
      if (light1.intensity >= 4) { 
        light1Increasing = false;
      }
    } else {
      light1.intensity -= 0.1; 
      if (light1.intensity <= 0.1) {
        light1Increasing = true;
      }
    }

    if (light2Increasing) {
      light2.intensity += 0.1; 
      if (light2.intensity >= 4) { 
        light2Increasing = false;
      }
    } else {
      light2.intensity -= 0.1; 
      if (light2.intensity <= 0.1) {
        light2Increasing = true;
      }
    }
  });
}

``` 
It checks if one of the lights has increased to its maximum. If it has, it will decrease. Once one light is dimmed, the other will reach its maximum, creating a continuous cycle.

```typescript 

function backgroundMusic(scene: Scene): Sound {
  let music = new Sound("music", "./assets/audio/rave.mp3", scene, null, {
    loop: true,
    autoplay: false,
  });

  scene.onBeforeRenderObservable.add(() => {
    if (!music.isPlaying) {
      music.play();
    }
  });

  scene.onDisposeObservable.add(() => {
    if (music.isPlaying) {
      music.stop();
    }
  });

  ```
  I also added music. If there are no rendered frames, the music will not play, but when the scene is rendered the music will play and stop if the scene is closed.

  ## Assets used
  
   
  [Music](https://pixabay.com/music/techno-trance-welcome-to-rave-174215/)
  [Monkey1](https://media.istockphoto.com/id/965307792/photo/chimpanzee-face.jpg?s=612x612&w=0&k=20&c=ubJK1XiTVeoM4u0t2c-L-CpFAEdiRa4dfXfmbpj4ubo=)
  [Monkey2](https://pngimg.com/image/18729)   







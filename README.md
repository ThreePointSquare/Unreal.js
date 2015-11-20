# Unreal.js

Unreal.js is a plug-in which brings V8-powered Javascript into UnrealEngine4. 

### Features
- Powered by latest V8 (ES6)
- CommonJS modules
- Full access to the whole UnrealEngine API
- Free to subclass existing classes including blueprint
- Web-dev like UMG (Jade, pseudo-css, pseudo-angular.js)
- Live reload
- Communicate with outer world: REST, process(pipe), arraybuffer, ...
- Bridge API for editor extension
- Auto-completion for Visual Studio (auto-generated *.d.ts)
- Dedicated Javascript console on UnrealEditor

  ![](https://github.com/ncsoft/Unreal.js/blob/master/doc/images/UnrealJs_JavascriptConsole.gif) 

### Install
- Install git-lfs first to download *.umap, *.uasset properly. (https://git-lfs.github.com/)
- Download prebuilt V8 and unzip into Engine/Plugins/UnrealJS. (files are located in *releases*)
- Open `Examples/JavascriptPlayground.uproject`

### License
Apache2

### Examples

#### Unreal 2048
- https://github.com/gabrielecirulli/2048

  ![](https://github.com/ncsoft/Unreal.js/blob/master/doc/images/UnrealJs_example_2048.gif)


#### Create a new actor
```js
let myActor = new Actor(GWorld,{X:10,Y:20,Z:30});
myActor.SetActorLocation({X:40,Y:80,Z:120});
```

#### Subclass an existing class
```js
class MyActor extends Actor {
  properties() {
    this.MyProp/*EditAnywhere+Replicated+int*/;
  }
  RPC(x/*int*/) /*Server+Reliable*/ {
    console.log('This function is replicated',This.MyProp++);
  }
}
let MyActor_C = require('uclass')()(global,MyActor);
if (GWorld.IsServer()) { 
  new MyActor_C(GWorld);
}
```

#### Node.js like 
```js
let _ = require('lodash');
let kick = () => {
  console.log("Hello timer!",_.keys(this));
  setTimeout(kick,1000);
};
kick();
```

#### GameUI-dev like web-dev
```jade
div
	span.full
		Button.full
			text {{text}}
		div.full
			Button.full(fn.on-clicked="inc()")
				text {{count}}
			Button.full(fn.on-clicked="add()")
				text Click button above!
	span
		text.yellow >
		EditableText(Binding.Text='text',
			fn.on-text-changed='text = ^arguments[0]',
			HintText="Your secret goes here")
		
	list.full(repeat='item in items',on-click="discard(item)") 
		HorizontalBox.small
			text.full {{item.key}}
			text.full {{item.value}}
```

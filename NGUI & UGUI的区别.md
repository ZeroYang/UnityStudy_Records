#NGUI & UGUI的区别

NGUI 第三方

UGUI Unity

##UGUI的Atlas和NGUI的Atlas的区别
NGUI是必须先打出图集然后才能开始做界面。这一点很烦，因为始终都要去考虑你的UI图集。比如图集会不会超1024 ，图集该如何来规划等等。而UGUI的原理则是，让开发者彻底模糊图集的概念，让开发者不要去关心自己的图集。做界面的时候只用小图，而在最终打包的时候unity才会把你的小图和并在一张大的图集里面。然而这一切一切都是自动完成的，开发者不需要去care它。

##Assetbundle

Resources目录下的可以通过Resources.load加载

读取Assetbundle
AssetBundle.CreateFromFile(Application.streamingAssetsPath +"/Main.assetbundle");
---
layout: post
title:  "unity shader 渲染之基本语法和结构"
categories: unity
tags: unity shader
---

* content
{:toc}

## 介绍

shaderlib是Unity引擎用来组织渲染脚本。

## SubShader

一个shader是由多个subshader组成，一个subshader可理解为一个shader的渲染方案。即subshader是针对不同的显卡而编写的，每一个shader至少有1个subshader、理论可以无线多个，但是一般两到三个足够。

```  java
Shader "Custom/TestName" {
    SubShader {
    }
    SubShader {
    }
}
```





## SubShader Tags

SubShader内部可以有标签(Tags)定义，Tag指定了这个SubShader的渲染顺序(时机)，以及其他一些设置。

``` java
Shader "Custom/TestName"{
    SubShader{
        Tags { "Queue"="Geometry+10" "RenderType"="Opaque" }
    }
}
```

+ "Queue"标签, 定义渲染顺序。默认值是为Background=1000,Geomery=2000,AlphaTest=2450,Transparent=3000,Overlay=4000也可以设置指定值。

+ "RenderType"标签, Unity可以运行时替换符合特定RenderType的所有shader, Camera.RenderWithShader或者Camera.SetReplacementShader配合使用，Unity内置的RenderType包括："Opaque"、"Transparent"、"Background"、"Overlay"。用户也可以定义任意自己的RenderType字符串。

+ "ForceNoShadowCasting"标签，值为"true"时，表示不接收阴影。

+ "IgnoreProjector"标签，值为"true"时，表示不接收Projector组件的投影。


## Pass

一个subshader（渲染方案）是由一个个pass块来执行的，每一个pass都会消耗对应一个DrawCall,在满足渲染条件的情况下，尽量减少pass的数量。

``` java
Shader "Custom/TestName"{
    SubShader{
        Pass{
        }
    }
}
```

Pass 具体取值可参考 [ShaderLab syntax: Pass](https://docs.unity3d.com/Manual/SL-Pass.html)

## Pass Tags

与subshader有自己专属的Tag类似，Pass也有Pass专属的Tag,其中最重要的Tag是"LightMode",指定Pass和Unity的哪一种渲染路径（"Rendering Path"）搭配使用。LightMode值包括Always=永远都渲染，但不处理光照，ShadowCaster=用于渲染产生的阴影物体，ShadowCollector=用于收集阴影到屏幕坐标Buff里。

``` java
Shader "Custom/TestName"{
    SubShader{
        Pass{
            Tags { "LightMode"="ForwardBase" }
        }
    }
}
```

Pass 具体取值可参考 [ShaderLab syntax: Pass Tags](https://docs.unity3d.com/Manual/SL-PassTags.html)

## FallBack

当硬件找不到所有Subshader执行时，便会选择执行FallBack。

``` java
Shader "Custom/TestName"{
    SubShader{
        Pass{
        }
    }
    FallBack "Diffuse"
}
```

## Properties

shaderlab允许使用者定义一些参数，这些参数可以在编辑器窗口动态设置也可以通过代码进行设置。常用的参数有一下几个方面。

``` java
Shader "Custom/TestName"{
    Properties{
        _Range("My Range", Range(0, 1)) = 0.7
        _Color("My Color", Color) = (1,1,1,1)
        _Tex("My Texture", 2D) = "" {}
        _Rect("My Rectangle", Rect) = "name" {}
        _Cube("My Cubemap", Cube) = "name" {}
        _Float("My Float", Float) = 1
        _Vector("My Vector", Vector) = (1,2,3,4)
    }

    SubShader {
        Pass{
            CGPROGRAM
            uniform float4 _Color;
            ENDCG
        }
    }

    SubShader{
        Pass{
            Color[_Color]
        }
    }
}
```

## Instruction

shaderlib除了可编程的不部分，也提供的一些渲染指令，用来控制渲染流程
中的一些关键步骤。一个完成shader编程类一样应该有属性、方法、指令、标
记、回调等组成，下面给出一个完成的shader模板。

``` java
Shader "Custom/TestName"{
	Properties{
		_Color("Color", Color) = (1,1,1,1)
		_MainTex("Main Tex", 2D) = "" {}
	}

	SubShader{
		Tags{ "Queue" = "Geometry" "IgnoreProjector" = "True" "RenderType" = "Opaque" }
		LOD 200

		Cull Back

		ZTest LEqual
		ZWrite On

		//Blend SrcAlpha OneMinusSrcAlpha

		CGINCLUDE
		#include "UnityCG.cginc"

		float4 _Color;
		sampler2D _MainTex;

		struct v2f {
			float4 vertex:SV_POSITION;
			float2 uv:TEXCOORD0;
		};

		v2f vert(appdata_base v) {
			v2f o;
			o.vertex = UnityObjectToClipPos(v.vertex);
			o.uv = v.texcoord;
			return o;
		}

		fixed4 frag(v2f i) : Color {
			half4 c = tex2D(_MainTex, i.uv);
			return c * _Color;
		}
		ENDCG

		Pass {
			Tags { "LightMode" = "ForwardBase" }

			CGPROGRAM
			#pragma vertex vert
			#pragma fragment frag
			ENDCG
		}
	}
	Fallback "Legacy Shaders/VertexLit"
}

```

## 参考资料

[Unity Shader Reference](https://docs.unity3d.com/Manual/SL-Reference.html)


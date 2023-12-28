---
layout: single
title:  "Sin(x)_shader"
categories: shader
Tag: [Unity, Shader]
---

![Alt text](<Untitled video - Made with Clipchamp (2)-1.gif>)


## using sine to move the vertex and frag

```
Shader "Custom/Interpolate_Training"
{
    
Properties
{   

    _TintColor("Test Color", color) = (1, 1, 1, 1)
_Intensity("Range Sample", Range(0, 1)) = 0.5
_MainTex("Main Texture", 2D) = "white" {}

[NoScaleOffset] _Flowmap("Flowmap", 2D) = "white" {}

_FlowTime("Flow Time",float) =0.5
_FlowIntensity("Flow Intensity",Float) =0.5

}  

SubShader
{  	
Tags
{
"RenderPipeline"="UniversalPipeline"
"RenderType"="Opaque"          
"Queue"="Transparent"		
}
Pass
{
Name "Universal Forward"
Tags {"LightMode" = "UniversalForward"}

HLSLPROGRAM
#pragma prefer_hlslcc gles
#pragma exclude_renderers d3d11_9x

#pragma vertex vert
#pragma fragment frag		

#include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/Lighting.hlsl"

half4 _TintColor;
float _Intensity;
float _FlowTime; float _FlowIntensity;

float4 _MainTex_ST;
Texture2D _MainTex;Texture2D _FlowMap;
SamplerState sampler_MainTex;	

struct VertexInput
{
  float4 vertex   : POSITION;
  float2 uv 	  : TEXCOORD0;
};

struct VertexOutput
{
 float4 vertex  	: SV_POSITION;
 float2 uv      	: TEXCOORD0;
 float3 color       : COLOR;
 
};

VertexOutput vert(VertexInput v)
{
    VertexOutput o;

    float4 PositionWS =  TransformObjectToHClip(v.vertex.xyz);
    float3 color = TransformObjectToWorld(v.vertex.xyz);

    o.vertex = PositionWS + float4(sin(color+_Time.z),1);
    o.color = color;
    return o;
}

half4 frag(VertexOutput i) : SV_Target
{
    
float4 color = float4(1, 1, 1, 1);
color.rgb *= _TintColor * _Intensity * i.color ;
return color;

}
ENDHLSL  
}
}

}
```
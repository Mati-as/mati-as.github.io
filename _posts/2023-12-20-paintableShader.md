---
layout: single
title:  "PaintableTexture"
categories: shader
---
<img src="https://github.com/Mati-as/mati-as.github.io/assets/120005151/b704f4f9-1548-415f-bfba-e89bd407c84e" width="500" height="500">
<img src="https://github.com/Mati-as/mati-as.github.io/assets/120005151/cb129577-9a3e-444f-8528-1886c7b49328" width="500" height="500">
<img src="https://github.com/Mati-as/mati-as.github.io/assets/120005151/d216b5a6-4b6e-44f3-ba97-9348224ed916" width="500" height="500">


### Shdaer Code 
 ```
Shader "Custom/PaintShader"
{
  Properties
    {
        _MainTex ("Texture", 2D) = "white" {}
        _MouseUV ("Mouse UV", Vector) = (-1,-1,0,0)
        _BrushSize ("Brush Size", Float) = 0.1
        _BrushStrength ("Brush Strength", Float) = 0
        _Color ("Color", Color) = (1,1,1,1)
    }
    SubShader
    {
       Tags{"RenderPipeline"= "UniversalPipeline"  "RenderType"= "Transparent" "RenderQueue" = "Transparent"}
        
        LOD 100
        Pass
        {
            Blend SrcAlpha OneMinusSrcAlpha
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "UnityCG.cginc"

            struct appdata
            {
                float4 vertex : POSITION;
                float2 uv : TEXCOORD0;
            };

            struct v2f
            {
                float2 uv : TEXCOORD0;
                float4 vertex : SV_POSITION;
            };
            float4 _MouseUV;
            float _BrushStrength;
            sampler2D _MainTex;
            float _BrushSize;
            float4 _Color;

            v2f vert (appdata v)
            {
                v2f o;
                o.uv = v.uv;
                o.vertex = UnityObjectToClipPos(v.vertex);
                return o;
            }

            fixed4 frag (v2f i) : SV_Target
            {
                fixed4 col = tex2D(_MainTex, i.uv);
                
               

                
                float dist = distance(i.uv, float2(_MouseUV.x, _MouseUV.y));
              
                if (dist < _BrushSize)
                {
                   
                    
                   col.a = lerp(col.a, 0, _BrushStrength); // Make the pixel more transparent based on the brush strength
                }

                return col;
            }
            ENDCG
        }
    }
   
 
  
}
```

### Unity Script

```
using UnityEngine;
using UnityEngine.InputSystem;

public class Painting_PaintableTextureController : MonoBehaviour
{
    public Shader paintShader;
    private Material paintMaterial;
    private RenderTexture renderTexture;
    private MeshRenderer _meshRenderer;
    public Texture2D textureToPaint;
    public float brushSize = 0.1f;
    public InputAction paintAction; // Define an InputAction for painting


     [Header("Shader Seting")] public float burshStrength;
    void Start()
    {
        renderTexture = new RenderTexture(textureToPaint.width, textureToPaint.height, 0, RenderTextureFormat.ARGB32);
        paintMaterial = new Material(paintShader);
        
        
        // Copy the original texture to the RenderTexture
        Graphics.Blit(textureToPaint, renderTexture);

        // Set the material's texture to the RenderTexture
        GetComponent<MeshRenderer>().material.mainTexture = renderTexture;

        // Initialize the paint action
        paintAction = new InputAction(binding: "<Mouse>/leftButton", interactions: "Press");
        paintAction.performed += ctx => StartPainting();
        paintAction.Enable();
    }

    void StartPainting()
    {
        RaycastHit hit;
        if (Physics.Raycast(Camera.main.ScreenPointToRay(Mouse.current.position.ReadValue()), out hit))
        {
           
            if (hit.transform == transform)
            {
                paintMaterial.SetFloat("_BrushStrength",burshStrength );

                Vector2 uv = hit.textureCoord;
                // Convert to "_MouseUV" for the shader
                paintMaterial.SetVector("_MouseUV", new Vector4(uv.x, uv.y, 0, 0));
                paintMaterial.SetFloat("_BrushSize", brushSize);

                RenderTexture temp = RenderTexture.GetTemporary(renderTexture.width, renderTexture.height, 0, RenderTextureFormat.ARGB32);
                Graphics.Blit(renderTexture, temp, paintMaterial);
                Graphics.Blit(temp, renderTexture);
                RenderTexture.ReleaseTemporary(temp);
            }
        }
    }

    private void OnEnable()
    {
        paintAction.Enable();
    }

    private void OnDisable()
    {
        paintAction.Disable();
    }
}
```

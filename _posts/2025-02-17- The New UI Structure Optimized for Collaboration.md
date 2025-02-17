---
layout: single
title:  "UI Structure Optimized for Collaboration"
categories: "Unity Tips"
tag: [Unity]
toc: true
sidebar:
    nav: "counts"
---

## Enhancing UI Management in Unity for Better Collaboration

Maintaining modularity and reusability is crucial, especially in team environments. However, collaboration between UI designers and programmers can be challenging since they have different needs regarding modularization. A module that is convenient for programmers may not be efficient for UI designers. The following BaseUI class provides a structured approach to managing UI elements, making it easier for teams to collaborate efficiently.

### Overview

The BaseUI class serves as a foundation for UI elements, handling initialization, animation, and event-driven callbacks for UI interactions. By implementing this class, developers can ensure a consistent workflow when working with UI components.

### Key Features

Modular Initialization

Init(Transform anchor): Configures UI elements by setting their parent and adjusting their position and scale.

Event-Driven UI Management

SetInfo(BaseUIData uiData): Assigns event callbacks (OnShow, OnClose) for when the UI is displayed or hidden.

Animation and Interaction Handling

ShowUI(): Plays an animation (if assigned) and triggers the OnShow callback.

CloseUI(bool isCloseAll = false): Closes the UI and invokes the OnClose callback unless all UI elements are being closed.

OnClickCloseButton(): Closes the UI while also playing a button click sound effect.

```
using System;
using UnityEngine;

public class BaseUIData
{
    public Action OnShow;
    public Action OnClose;
}

public class BaseUI : MonoBehaviour
{
    public Animation m_UIOpenAnim;

    private Action m_OnShow;
    private Action m_OnClose;

    public virtual void Init(Transform anchor)
    {
        Logger.Log($"{GetType()}::Init");

        m_OnShow = null;
        m_OnClose = null;

        transform.SetParent(anchor);

        var rectTransform = GetComponent<RectTransform>();
        rectTransform.localPosition = Vector3.zero;
        rectTransform.localScale = Vector3.one;
        rectTransform.offsetMin = Vector2.zero;
        rectTransform.offsetMax = Vector2.zero;
    }

    public virtual void SetInfo(BaseUIData uiData)
    {
        Logger.Log($"{GetType()}::SetInfo");

        m_OnShow = uiData.OnShow;
        m_OnClose = uiData.OnClose;
    }

    public virtual void ShowUI()
    {
        if(m_UIOpenAnim)
        {
            m_UIOpenAnim.Play();
        }

        m_OnShow?.Invoke();
        m_OnShow = null;
    }

    public virtual void CloseUI(bool isCloseAll = false)
    {
        if(!isCloseAll)
        {
            m_OnClose?.Invoke();
        }
        m_OnClose = null;

        UIManager.Instance.CloseUI(this);
    }

    public virtual void OnClickCloseButton()
    {
        AudioManager.Instance.PlaySFX(SFX.ui_button_click);
        CloseUI();
    }
}
```
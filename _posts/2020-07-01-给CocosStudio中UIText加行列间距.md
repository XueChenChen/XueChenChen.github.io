---
layout:     post
title:      "UIText行列间距"
date:       2020-07-1 20:29:22
author:     "Chen"
header-img: "img/skillTable/skillTable-bg.jpg"
tags:
    - cocos
    - cocosStudio
    - UIText
    - lua
---

1. 本身UIText也是靠CCLabel来渲染的
```cpp
	void Text::initRenderer()
{
    _labelRenderer = Label::create();
    addProtectedChild(_labelRenderer, LABEL_RENDERER_Z, -1);
}
```
2. CCLabel 有设置列间距行间距的方法
```cpp
	// 行间距
	void setLineSpacing(float height);

	/**
	 * Sets the additional kerning of the Label.
	 *
	 * @warning Not support system font.
	 * @since v3.2.0
	 *  列间距
	 */
	void setAdditionalKerning(float space);
```

3. 在 C++ 里 添加 行间距 列间距 方法 然后暴露给lua


- UIText中添加方法

```cpp
// UIText.h 中

	//设置行间距
	void setLineSpacing(float height);
	//设置列间距
	void setAdditionalKerning(float dir);

// UIText.cpp 中

	//设置行间距
	void Text::setLineSpacing(float height)
	{
	    if (_labelRenderer == nullptr)
	        return;
	    
	    _labelRenderer->setLineSpacing(height);

	}
	//设置列间距
	void Text::setAdditionalKerning(float dir)
	{
	    if (_labelRenderer == nullptr)
	        return;
	        
	    _labelRenderer->setAdditionalKerning(dir);
	}

```
- 暴露给lua

**文件路径** `cocos2d-x/cocos/scripting/lua-bindings/auto/lua_cocos2dx_ui_auto.cpp` <br>
**方法** `lua_register_cocos2dx_ui_Text`

```cpp
// 行间距
int lua_cocos2dx_ui_Text_setLineSpacing(lua_State* tolua_S)
{
    int argc = 0;
    cocos2d::ui::Text* cobj = nullptr;
    bool ok  = true;

#if COCOS2D_DEBUG >= 1
    tolua_Error tolua_err;
#endif


#if COCOS2D_DEBUG >= 1
    if (!tolua_isusertype(tolua_S,1,"ccui.Text",0,&tolua_err)) goto tolua_lerror;
#endif

    cobj = (cocos2d::ui::Text*)tolua_tousertype(tolua_S,1,0);

#if COCOS2D_DEBUG >= 1
    if (!cobj) 
    {
        tolua_error(tolua_S,"invalid 'cobj' in function 'lua_cocos2dx_ui_Text_setLineSpacing'", nullptr);
        return 0;
    }
#endif

    argc = lua_gettop(tolua_S)-1;
    if (argc == 1) 
    {
        double arg0;

        ok &= luaval_to_number(tolua_S, 2,&arg0, "ccui.Text:setLineSpacing");
        if(!ok)
        {
            tolua_error(tolua_S,"invalid arguments in function 'lua_cocos2dx_ui_Text_setLineSpacing'", nullptr);
            return 0;
        }
        cobj->setLineSpacing(arg0);
        lua_settop(tolua_S, 1);
        return 1;
    }
    luaL_error(tolua_S, "%s has wrong number of arguments: %d, was expecting %d \n", "ccui.Text:setLineSpacing",argc, 1);
    return 0;

#if COCOS2D_DEBUG >= 1
    tolua_lerror:
    tolua_error(tolua_S,"#ferror in function 'lua_cocos2dx_ui_Text_setLineSpacing'.",&tolua_err);
#endif

    return 0;
}
// 列间距
int lua_cocos2dx_ui_Text_setAdditionalKerning(lua_State* tolua_S)
{
    int argc = 0;
    cocos2d::ui::Text* cobj = nullptr;
    bool ok  = true;

#if COCOS2D_DEBUG >= 1
    tolua_Error tolua_err;
#endif


#if COCOS2D_DEBUG >= 1
    if (!tolua_isusertype(tolua_S,1,"ccui.Text",0,&tolua_err)) goto tolua_lerror;
#endif

    cobj = (cocos2d::ui::Text*)tolua_tousertype(tolua_S,1,0);

#if COCOS2D_DEBUG >= 1
    if (!cobj) 
    {
        tolua_error(tolua_S,"invalid 'cobj' in function 'lua_cocos2dx_ui_Text_setAdditionalKerning'", nullptr);
        return 0;
    }
#endif

    argc = lua_gettop(tolua_S)-1;
    if (argc == 1) 
    {
        double arg0;

        ok &= luaval_to_number(tolua_S, 2,&arg0, "ccui.Text:setAdditionalKerning");
        if(!ok)
        {
            tolua_error(tolua_S,"invalid arguments in function 'lua_cocos2dx_ui_Text_setAdditionalKerning'", nullptr);
            return 0;
        }
        cobj->setAdditionalKerning(arg0);
        lua_settop(tolua_S, 1);
        return 1;
    }
    luaL_error(tolua_S, "%s has wrong number of arguments: %d, was expecting %d \n", "ccui.Text:setAdditionalKerning",argc, 1);
    return 0;

#if COCOS2D_DEBUG >= 1
    tolua_lerror:
    tolua_error(tolua_S,"#ferror in function 'lua_cocos2dx_ui_Text_setAdditionalKerning'.",&tolua_err);
#endif

    return 0;
}

int lua_register_cocos2dx_ui_Text(lua_State* tolua_S)
{
	//***
	tolua_function(tolua_S,"setLineSpacing",lua_cocos2dx_ui_Text_setLineSpacing);
	tolua_function(tolua_S,"setAdditionalKerning",lua_cocos2dx_ui_Text_setAdditionalKerning);
	//***
}
```

------------ 进阶 ------------------

策划像在CocosStudio中Text的高级属性配置用户数据
![IMG](/img/cocos/CocosStudio_userData.jpg)

csd 内容
![IMG](/img/cocos/CocosStudio_userData2.jpg)

code
```lua
local exData = text:getComponent("ComExtensionData")
local data = exData:getCustomProperty();
// data 就是 999999
```

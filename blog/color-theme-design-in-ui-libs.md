# Color Theme Desgin in UI Libs

When we use a UI lib, we always want to change some style to fit our app. It is necessary for UI libs to provide a standard way for developers to override default theme. For me, I think it is useful to divide the lib style into 3 levels, which are `component inner UI logic (**CIUL** in short in this article)`, `lib design specification` and `biz style`.

## Level 1, CIUL (component inner UI logic)

**What is CIUL?**

When we use a component, each style element(e.g. background color, text color, border color, font size, padding, etc.) of the component is related in a way. For example, a button background is **lightened** when the mouse is hovered over it, and **darkend** when it is activited. The `lighten` and `darken` variation is regarded as the `inner UI logic` for the button component. Another example is in a fill mode button, border color is same with __background color__, and in a outline mode button, border color is same with __text color__. Background color, text color, border color, they are related in a button component, no matter what color theme it might be.

**Why we need CIUL?**

Actually, we couldn't say we need it, but it exists natually. When we say a UI lib, it must be a mount of components in some designed style. So, there are features in common that we could thus tell it a union or in a same style group. The inner UI logic is the style's core. That means, in our lib, we always lighten the background color when a component is hovered, and darken when activited. The same variation logic makes these component a union in the same style. Thus, we can't say "we need the CIUL", but say "we can't ignore it's existence".

**How to define a CIUL?**

We need to define **relations** among UI elements in a component, Such as relations among background color, border color and text color in button example. CIUL should satisfy user experience and user interface needs. 


## Level 2, lib design specification

In a well designed UI lib, a design specification is necessary. Samilar with CIUL, it also privide a unioned feeling in a more common way. For example, we set all primary color to '#1792ff' whitch is a value of blue in our design spec. Thus, all components are _blued_. When using this lib with a _blued_ design spec, we could feel it more commercial. Well, if we set this primary color brown, we could feel it more old school. Not only we set base colors in design spec, but font size, font weight, margin, padding, shadow, etc. These are base elements of style, we set them in design spec and use them in CIUL.

## Level 3, biz style

Actually, libs only contain two levels above(CIUL and design spec). So, what is level 3 for? We know, a UI lib is for using in biz projects. And we may also know, a biz project is never satisfied by a UI lib itself, it need more variation in style. So that's why I place level 3 here. When we split a design to code, we should decide which component we should use. We normally make this decision by two facts: function and style.

**Function**

We will never use an actionSheet component as a link, because it's not suit in function. Functionality tells us the right component basically. Sometimes, there are choices. For example, we need a clickable text as a link trigger. What component could we use? An HTML `a` element? A plain button component? Or we have a link component? 

It seems an `a` element is a good choice. It might be. Let's see what's going on when we use it. We can easily set its href property and wrap a text string within it, thus we got a link. It's easy right? We decorate it in a CSS file and it's getting beautiful. It's so classic a way. But if we want more extensions, it may not a good choice.

What will happen if we use a plain button component? 
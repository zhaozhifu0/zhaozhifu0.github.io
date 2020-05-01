---
title: 【翻译】 第六篇 Atomic Design with React
toc: true
tags:
  - 翻译
  - React
  - 设计方法
categories:
  - 翻译
abbrlink: 3709c998
date: 2019-11-30 15:59:24
---


<center><h1> React 的原子设计</h1></center>

> 本文来自阅读极客时间专栏《左耳听风》89 | 程序员练级攻略：UI/UX设计 文章中的推荐阅读

- 原文链接：https://codeburst.io/atomic-design-with-react-e7aea8152957
- 作者：Danilo Woznica
- 2018年1月9日

![](/assets/translate/201912042105.jpg)

How the Atomic Design methodology allowed me to create a great design system from scratch and made me a better developer, with principles of componentization, hierarchies and reuses of code.

原子设计方法论是如何让我从零开始创建一个伟大的设计系统，并使我成为一个更好的开发人员，具有组件化、层次结构和代码重用的原则。
<!--more-->
I have recently had the opportunity to work on a new product from scratch made in React and PWA with the well-crafted and componentized UI at Cheesecake. However, when we discussed with the whole team the best way to approach the development, we ended up having the same old problems that have happened in most past projects, such as:

我最近有机会在一个新的产品从零开始的反应和 PWA 与精心制作和组件化的 UI 在芝士蛋糕。 然而，当我们与整个团队讨论处理开发的最佳方法时，我们最终遇到了与过去大多数项目中发生的相同的老问题，例如:

- Lack of Styleguide of components; 缺乏构件的样式指南;
- Lack of precision in estimating development time; 估计开发时间不精确;
- Great amount of setup time for developers; 为开发人员提供大量的安装时间;
- Inconsistency between components and view; 组件与视图不一致;
- Repeated code; 重复代码;
- Side effects; 副作用;
- Very specific components to each pages; 每个页面都有非常具体的组件;

We’ve started to build the CSS architecture using the ITCSS methodology that organizes the style files on stacks from generics styles to the specifics ones, which helps you to scale large projects easily. But along with ITCSS, we were using CSS Modules to scope the components, so we noticed that component stack was getting huge and even the generic styles were being componentized and reused within other one.

我们已经开始使用 ITCSS 方法来构建 CSS 架构，这种方法将样式文件从泛型样式组织到具体样式，这有助于您轻松地扩展大型项目。 但是与 ITCSS 一起，我们使用 CSS 模块来调整组件的范围，所以我们注意到组件堆栈变得越来越庞大，甚至通用样式也被组件化并在其他组件中重用。

That was the moment in which we paused to rethink our architecture and how we could set the components in a more distributed and organized way. Then we found a methodology called Atomic Design that creates multiples stacks of components, with different hierarchies of complexity and dependence.

在那一刻，我们停下来重新思考我们的体系结构，以及我们如何将组件设置成更加分布和有组织的方式。 然后我们发现了一种叫做原子设计的方法，它创建了多个组件堆栈，具有不同的复杂性和依赖性层次结构。


![原子设计层次结构](/assets/translate/201911301609.png "原子设计层次结构")

## What is Atomic Design?

## 什么是原子设计？

Popularly known within the design world, Atomic Design helps to build consistent, solid and reusable design systems. Plus, in the world of React, Vue and frameworks that stimulate the componentization, Atomic Design is used unconsciously; but when used in the right way, it becomes a powerful ally for developers.

原子设计在设计界广为人知，它帮助构建一致、可靠和可重用的设计系统。 另外，在 React、 Vue 和促进组件化的框架的世界中，原子设计是无意识地使用的; 但是当使用得当时，它成为了开发人员的强大盟友。

The name Atomic Design comes from the idea of separating the components in atoms, molecules, organisms, templates and pages, like in the image above. But what are the responsibilities of each separated part?

“原子设计”这个名字来源于这样一个想法，即将原子、分子、有机体、模板和页面中的成分分离开来，如上图所示。 但是，每个独立部分的责任是什么？

### Atoms
### 原子

![原子组件示例](/assets/translate/201911301613.png "原子组件示例")

Atoms are the smallest possible components, such as buttons, titles, inputs or event color pallets, animations, and fonts. They can be applied on any context, globally or within other components and templates, besides having many states, such as this example of button: disabled, hover, different sizes, etc.

原子是最小的可能组件，例如按钮、标题、输入或事件颜色托盘、动画和字体。 它们可以应用于任何上下文，全局或其他组件和模板，除了有许多状态，如这个按钮的例子: 禁用，悬停，不同大小等。

### Molecules
### 分子

![分子组成的例子](/assets/translate/201911301615.png "分子组成的例子")

They are the composition of one or more components of atoms. Here we begin to compose complex components and reuse some of those components. Molecules can have their own properties and create functionalities by using atoms, which don’t have any function or action by themselves.
它们是由一个或多个原子组成的。 在这里，我们开始组合复杂的组件并重用其中的一些组件。 分子可以有自己的属性，通过原子创造功能，而原子本身没有任何功能或作用。

### Organisms
### 有机体

![生物成分的例子](/assets/translate/201911301635.png "生物成分的例子")

Organisms are the combination of molecules that work together or even with atoms that compose more elaborate interfaces. At this level, the components begin to have the final shape, but they are still ensured to be independent, portable and reusable enough to be reusable in any content.

有机体是分子的组合，这些分子相互作用，甚至与原子组成更复杂的界面。 在这个级别上，组件开始具有最终的形状，但仍然保证它们是独立的、可移植的和可重用的，足以在任何内容中重用。

### Templates
### 模板

![](/assets/translate/201911301636.png)

In this state we stop composing components and begin to set their context. Moreover, the templates create relationships between the organisms and others components through positions, placements and patterns of the pages but it doesn’t have any style, color or component rendered. That’s why it looks like a wireframe.
在这种状态下，我们停止组合组件并开始设置它们的上下文。 此外，模板通过页面的位置、位置和模式在有机体和其他组件之间创建关系，但它没有任何风格、颜色或组件渲染。 这就是为什么它看起来像线框图。

### Pages
### 页数

![](/assets/translate/201911301638.png)

Pages are the navigate parts of the application and it’s where the components are distributed in one specific template. The components get real content and they’re connected with the whole application. At this stage, we can test the efficiency of the design system to analyse if all the components are independent enough or if we need to split them in smaller parts.

页面是应用程序的导航部分，它是组件在一个特定模板中分布的位置。 这些组件获得真正的内容，并与整个应用程序相连接。 在这个阶段，我们可以测试设计系统的效率，以分析是否所有的组件都足够独立，或者是否需要将它们拆分成更小的部分。

### React + Atomic Design
### React + 原子设计

When we started to use Atomic Design within React we had to adjust some rules of the methodology to ensure that components were reused as much as possible, that they were stateless, without styles of positions and very specific margins so to avoid any side effects in the pages of application.
当我们开始在 React 中使用 Atomic Design 时，我们不得不调整方法的一些规则，以确保组件被尽可能多地重用，确保它们是无状态的，没有位置样式和非常具体的边距，以避免应用程序页面中的任何副作用。

So with each new component we asked ourselves: “Are these components generic enough to avoid specificity and/or repeated code in whatever context they are used?”
因此，对于每一个新组件，我们都要问自己: “这些组件是否足够通用，以避免在任何情况下使用特殊性和 / 或重复代码? ”

So we were able to write a few rules:
所以我们可以写一些规则:

1. The Atomic Design should have a file of variables and it must be imported by each component;
1.原子设计应具有一个变量文件，并且必须由每个组件导入。

2. The atoms should be written without margins and positions;
2.原子应该写在没有边缘和位置的地方；

3. Only the molecules and organisms can set the positions of atoms, but these stacks can’t have any styles of margins and positions;
3.只有分子和有机体可以设置原子的位置，但这些堆栈不能有任何类型的边距和位置;

4. Templates have only one function: to set the grid of pages but never positions of specific components;
4.模板只有一个功能: 设置页面的网格，但不设置特定组件的位置;

5. Pages render the components with a template defined and it’s here that the Atomic Design will be connected to the rest of the application;
5.页面使用定义的模板呈现组件，在这里原子设计将连接到应用程序的其余部分;

### Let’s code
### 让我们来编码

What I will show here is everything in a boilerplate on GitHub, which you can test and then start your projects using Atomic Design, so let’s do it:
这里我要展示的是 GitHub 上的样板文件，你可以测试它，然后使用 Atomic Design 开始你的项目，所以让我们开始吧:

To build a UI LIbrary we used an awesome tool called Storybook, which is a great ally to the Atomic Design in React (you can use it for the React Native and Vue too), it allows to render the components and list all states/variations of one.
为了构建 UI LIbrary，我们使用了一个非常棒的工具 Storybook，它是原子设计在反应(你可以使用它为反应原生和 Vue 也)的一个很好的盟友，它允许渲染组件和列出一个的所有状态 / 变化。

With the Storybook installed the folder structure will look like this:
安装了 Storybook 后，文件夹结构如下:

![Structure folder](/assets/translate/201912042026.png)

Note that inside of the button component there is a file called ‘index.js’ which is the component itself, the ‘styles.css’ is the style that will be imported by CSS Modules (here we’ve used the BEM CSS inside the structure; I recommend reading the article ‘CSS Architecture with ReactJS’) and the ‘stories.js’ is the file that will import the component into the Storybook, which looks like this:
注意，在按钮组件的内部有一个叫做‘ index.js'的文件，它是组件本身，‘ styles.CSS'是由 CSS 模块导入的样式(这里我们在结构中使用了 BEM CSS; 我推荐阅读文章‘ CSS Architecture with ReactJS') ，而‘ stories.js'是将组件导入 Storybook 的文件，它看起来像这样:

![Storybook’s file](/assets/translate/201912042029.png)

Each ‘add ()’ function is a variation of the component and it is the best approach to describe each state individually rather than a single function, so it becomes easier to highlight and control each one of them. So if you describe all the component variations , the Storybook should look like this:
每个 add ()函数都是组件的一个变体，它是描述每个状态的最佳方法，而不是单个函数，因此更容易突出和控制每个状态。 因此，如果你描述所有的组件变化，故事书应该是这样的:

![Storybook interface](/assets/translate/201912042030.png)

And the coolest part of the Storybook is that you can add some ‘addons’, such as the Storybook Info, which does awesome things like: story source, props types, defaults values and which values are required or not.
故事书最酷的部分是你可以添加一些插件，比如故事书信息，它可以做一些很棒的事情，比如: 故事来源，道具类型，默认值以及哪些值是必需的或者不是。

![Storybook table](/assets/translate/201912042031.png)

### Conclusion
### 总结

At the end of project, we reached the initial goals and we believe we left a good legacy, a structure which ensures that the project can grow and that other developers can understand the architecture quickly. Maybe we initially spent some extra time writing stories and etc, but the more the project grows, the more the benefits make clear why we should use such architecture.
在项目结束时，我们达到了最初的目标，并且我们相信我们留下了一个很好的遗产---- 一个确保项目可以成长并且其他开发人员可以快速理解架构的结构。 也许我们最初花费了一些额外的时间来写故事等等，但是随着项目的增长，好处越来越明显，为什么我们应该使用这样的架构。

However we could see that this architecture probably doesn’t work for every project because it depends on several factors. The main one is that the design needs to be thought in the same way as the development: in an atomic way. But the integration between design and development is a point that every project wants to reach, so it becomes a very positive point for Atomic Design.
然而，我们可以看到，这种架构可能不适用于每个项目，因为它取决于几个因素。 主要的一点是，设计需要以与开发相同的方式来考虑: 以原子的方式。 但是设计和开发之间的集成是每个项目都想要达到的，所以这对于原子设计来说是一个非常积极的点。



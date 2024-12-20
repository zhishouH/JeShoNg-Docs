# 纯 css 实现网格背景

## 线性渐变

- 使用 `linear-gradient()` 函数实现网格，既需要横向的线条，也需要纵向的线条
- 使用 `background-size` 控制网格间隙大小

![image-20240131182111821](/public/sundries/纯css网格背景-完整.png)

::: code-group

```html [grid.html]
<div class="grid"></div>
```

```css [grid.css]
.grid {
	width: 1080px;
	height: 400px;
	background-image: linear-gradient(to right, #ccc 1px, transparent 1px),
		linear-gradient(to bottom, #ccc 1px, transparent 1px);
	background-size: 10px 10px;
	background-position: center;
	margin: 100px auto;
}
```

:::

### 1、边缘虚化

- 使用 `mask-image` 属性作为元素蒙版层的图像，[蒙版层文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/mask-image)，
- 使用 `radial-gradient` (径向渐变)实现
- 使用 `background-size` 控制网格间隙大小

![image-20240131182111821](/public/sundries/纯css网格背景-虚化.png)

::: code-group

```html [grid.html]
<div class="grid"></div>
```

```css [grid.css]
.grid {
	width: 1080px;
	height: 400px;
	background-image: linear-gradient(to right, #ccc 1px, transparent 1px),
		linear-gradient(to bottom, #ccc 1px, transparent 1px);
	mask-image: radial-gradient(
		ellipse 50% 50% at 50% 50%,
		#000 70%,
		transparent 100%
	);
	background-size: 10px 10px;
	background-position: center;
	margin: 100px auto;
}
```

:::

### 2、四周渐变

- 使用 `background-size` 控制网格间隙大小

![image-20240131182111821](/public/sundries/纯css网格背景-四周渐变.png)

::: code-group

```html [grid.html]
<div class="grid"></div>
```

```css [grid.css]
.grid {
	width: 500px;
	height: 200px;
	background-image: linear-gradient(to right, #ccc 1px, transparent 1px),
		linear-gradient(to bottom, #ccc 1px, transparent 1px);
	mask-image: linear-gradient(
			to bottom,
			transparent,
			#fff 50px calc(100% - 50px),
			transparent
		), linear-gradient(to right, transparent, #fff 50px calc(100% - 50px), transparent);
	mask-composite: intersect;
	background-size: 10px 10px;
	background-position: center;
	margin: 100px auto;
}
```

:::

## 网状点阵

- 使用 `radial-gradient` 函数，创建圆形填充背景色
- 使用 `background-size` 控制网格间隙大小

![image-20240131182111821](/public/sundries/纯css网状点阵背景.png)

::: code-group

```html [grid.html]
<div class="grid"></div>
```

```css [grid.css]
.grid {
	width: 500px;
	height: 200px;
	background-image: radial-gradient(circle, rgb(203 213 225) 2px, #fff 2px);
	background-size: 20px 20px;
	background-position: center center;
	margin: 100px auto;
}
```

:::

## 格子花纹

![image-20240131182111821](/public/sundries/纯css网格背景-格子花纹.png)

::: code-group

```html [grid.html]
<div class="grid"></div>
```

```css [grid.css]
.grid {
	width: 500px;
	height: 200px;
	background-image: linear-gradient(45deg, #8d8b8b 25%, transparent 0),
		linear-gradient(-45deg, #8d8b8b 25%, transparent 0), linear-gradient(
			45deg,
			transparent 75%,
			#8d8b8b 0
		), linear-gradient(-45deg, transparent 75%, #8d8b8b 0);
	background-position: 0 0, 0 10px, 10px -10px, -10px 0;
	background-size: 20px 20px;
}
```

:::

## 工具生成

<grid-background-generator @returnGridCss="handleGridCss"/>

<script setup>
	import { ref } from "vue"

	const gridCss = ref({})

	const size = ref(10)
	const handleGridCss = (e) => {
		gridCss.value = e
		console.log(1, e)
	}

	

</script>

<div v-if="gridCss.currentModeIndex === 1 && gridCss.isShowMask">

```css-vue
.grid {
	width: 500px;
	height: 200px;
	background-size: {{ gridCss.gridSize }}px {{ gridCss.gridSize }}px;
	background-position: center center;
	background-image: linear-gradient(to right, {{ gridCss.currnetColor }} {{ gridCss.lineWidth }}px, transparent {{ gridCss.lineWidth }}px),	linear-gradient(to bottom, {{ gridCss.currnetColor }} {{ gridCss.lineWidth }}px, transparent {{ gridCss.lineWidth }}px);
	mask-image: linear-gradient(to bottom, transparent, #fff {{gridCss.gradientRadius}}px calc(100% - {{gridCss.gradientRadius}}px), transparent),
            linear-gradient(to right, transparent, #fff {{gridCss.gradientRadius}}px calc(100% - {{gridCss.gradientRadius}}px));
	mask-composite: intersect;
}
```

</div>

<div v-if="gridCss.currentModeIndex === 1 && !gridCss.isShowMask">

```css-vue
.grid {
	width: 500px;
	height: 200px;
	background-size: {{ gridCss.gridSize }}px {{ gridCss.gridSize }}px;
	background-position: center center;
	background-image: linear-gradient(to right, {{ gridCss.currnetColor }} {{ gridCss.lineWidth }}px, transparent {{ gridCss.lineWidth }}px),	linear-gradient(to bottom, {{ gridCss.currnetColor }} {{ gridCss.lineWidth }}px, transparent {{ gridCss.lineWidth }}px);
}
```

</div>

<div v-if="gridCss.currentModeIndex === 2 && gridCss.isShowMask">

```css-vue
.grid {
	width: 500px;
	height: 200px;
	background-size: {{ gridCss.gridSize }}px {{ gridCss.gridSize }}px;
	background-position: center center;
	background-image: radial-gradient(circle, {{ gridCss.currnetColor}} {{ gridCss.lineWidth }}px, #fff {{ gridCss.lineWidth }}px);
	mask-image: linear-gradient(to bottom, transparent, #fff {{gridCss.gradientRadius}}px calc(100% - {{gridCss.gradientRadius}}px), transparent),
            linear-gradient(to right, transparent, #fff {{gridCss.gradientRadius}}px calc(100% - {{gridCss.gradientRadius}}px));
	mask-composite: intersect;
}
```

</div>

<div v-if="gridCss.currentModeIndex === 2 && !gridCss.isShowMask">

```css-vue
.grid {
	width: 500px;
	height: 200px;
	background-size: {{ gridCss.gridSize }}px {{ gridCss.gridSize }}px;
	background-position: center center;
	background-image: radial-gradient(circle, {{ gridCss.currnetColor}} {{ gridCss.lineWidth }}px, #fff {{ gridCss.lineWidth }}px);
}
```

</div>

<div v-if="gridCss.currentModeIndex === 3 && gridCss.isShowMask">

```css-vue
.grid {
	width: 500px;
	height: 200px;
	background-size: {{ gridCss.gridSize }}px {{ gridCss.gridSize }}px;
	background-position: 0 0, 0 {{ gridCss.gridSize / 2 }}px,
						{{ gridCss.gridSize / 2 }}px -{{ gridCss.gridSize / 2 }}px,
						-{{ gridCss.gridSize / 2 }}px 0;
	background-image: linear-gradient(45deg, {{ gridCss.currnetColor }} 25%, transparent 0),	linear-gradient(-45deg, {{ gridCss.currnetColor }} 25%, transparent 0), linear-gradient(45deg, transparent 75%, {{ gridCss.currnetColor }} 0), linear-gradient(-45deg, transparent 75%, {{ gridCss.currnetColor }} 0);
	mask-image: linear-gradient(to bottom, transparent, #fff {{gridCss.gradientRadius}}px calc(100% - {{gridCss.gradientRadius}}px), transparent), linear-gradient(to right, transparent, #fff {{gridCss.gradientRadius}}px calc(100% - {{gridCss.gradientRadius}}px), transparent);
	mask-composite: intersect;
}
```

</div>

<div v-if="gridCss.currentModeIndex === 3 && !gridCss.isShowMask">

```css-vue
.grid {
	width: 500px;
	height: 200px;
	background-size: {{ gridCss.gridSize }}px {{ gridCss.gridSize }}px;
	background-position: 0 0, 0 {{ gridCss.gridSize / 2 }}px,
						{{ gridCss.gridSize / 2 }}px -{{ gridCss.gridSize / 2 }}px,
						-{{ gridCss.gridSize / 2 }}px 0;
	background-image: linear-gradient(45deg, {{ gridCss.currnetColor }} 25%, transparent 0),	linear-gradient(-45deg, {{ gridCss.currnetColor }} 25%, transparent 0), linear-gradient(45deg, transparent 75%, {{ gridCss.currnetColor }} 0), linear-gradient(-45deg, transparent 75%, {{ gridCss.currnetColor }} 0);
}
```

</div>


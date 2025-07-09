
### Animation with 4s delay between iterations

```css
.animation-flash {
	animation: flash 5s linear infinite;
}

@keyframes flash {
    0% {
        left: -20%;
    }

    20%, 100% {
        left: 120%;
    }
}
```

### Pulse

```css 
.bar {
    position: relative;
	width: 50px;
	height: 50px;
	background-color: #faae41;
	border-radius: 50%;
}

.bar::before {
    content: '';
    position: absolute;
    z-index: -1;
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #faae41;
    opacity: 0.8;
    animation: pulse 2s ease-out infinite;
}

.bar::after {
    content: '';
    position: absolute;
    z-index: -1;
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #faae41;
    opacity: 0.8;
    animation: pulse 2s 1s ease-out infinite;
}

@keyframes pulse {
    100% {
        transform: scale(2.5); 
        opacity: 0;
    }
}
```
___

### Border out

```css 
.foo {
    position: relative;
	width: 120px;
	height: 30px;
	background-color: #faae41;
	border-radius: 2px;
	cursor: pointer;
}

.foo::after {
    content: '';
    position: absolute;
    z-index: -1;
    top: 50%;
    left: 50%;
    width: 100%;
    height: 100%;
    border: 2px solid #fff;
    border-radius: 2px;
    transform: translate(-50%, -50%);
    animation: border-out 4s ease-in-out infinite
}

@keyframes border-out {
    50% {
        transform: translate(-50%, -50%) scale(1.15, 1.7);
        border-width: 1px 1.8px 1px 1.8px;
        border-color: #fff;
        border-style: solid;
    }
}
```
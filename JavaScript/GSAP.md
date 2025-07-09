```js
const cardBackground1 = document.querySelector('.mosquito-cards__item-img-static--1');
        const cardAnimated1 = gsap.to(".mosquito-cards__item-img-animated--1", {
            // При использовании keyframes, установить repeat: 1
            // keyframes: {
            //  '0%': { x: 40, y: 50 },
            //  '40%': { x: 10, y: 10 },
            //  '50%': { x: 10, y: 10 },
            //  '90%': { x: 40, y: 50 },
            //  '100%': { x: 40, y: 50 },
            // },

            x: 9,
            y: 8,
            duration: 1.25,
            repeat: 1,
            yoyo: true,
            yoyoEase: true,
            paused: true,
            onComplete: checkHover,
        });

        let hover;

        function checkHover() {
            if (hover) cardAnimated1.restart();
        };

        function hoverOn() {
            hover = true;
            cardAnimated1.totalProgress() < 0.5 ? cardAnimated1.play() : cardAnimated1.restart();
        };

        function hoverOff() {
            hover = false;
            if (cardAnimated1.totalProgress() < 0.5) cardAnimated1.reverse();
        }

        cardBackground1.addEventListener("mouseenter", () => hoverOn());
        cardBackground1.addEventListener("mouseleave", () => hoverOff());
```
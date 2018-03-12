# Mega Bounce

```blocks
game.splash("Juggler", "Press arrows")

game.setBackground(3);
// images
let balls: Sprite[] = []

function addBall(ay: number) {
    let ball = sprites.create(img`
...6
..626
.62926
.62226
..626
...6
`)
    ball.image.replace(6, Math.randomRange(1, 15))
    ball.image.replace(2, Math.randomRange(1, 15))
    ball.ay = ay
    balls.push(ball);
}

let paddle = sprites.create(img`
2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
`)
paddle.y = screen.height - 10
let counter = 0

addBall(80);

loops.frame(function () {
    counter++;
    for (const ball of balls) {
        if (ball.collidesWith(paddle)) {
            ball.vy = -20 - ball.ay;
            ball.vx = Math.randomRange(-40, 40)
            hud.changeScoreBy(1)
            if (hud.score() % 5 == 0)
                addBall(ball.ay);
        }
        if (ball.right > screen.width || ball.left < 0)
            ball.vx = -ball.vx;
        if (ball.y > screen.height)
            game.over();
        if (counter % 10 == 0)
            ball.ay++;
    }
    paddle.x = Math.clamp(paddle.width / 2, screen.width - paddle.width / 2,
        paddle.x + keys.dx(180))
})
```
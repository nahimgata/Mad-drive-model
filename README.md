# Mad-drive-model<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Drive Mad Clone</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.6.0/p5.js"></script>
    <script src="https://unpkg.com/@liabru/matter-js"></script>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <script src="game.js"></script>
</body>
</html>
body {margin: 0;
    overflow: hidden;
    background-color: #87CEEB;}let Engine = Matter.Engine,
    World = Matter.World,
    Bodies = Matter.Bodies;

let engine, world;
let car, ground, obstacles = [];

function setup() {
    createCanvas(windowWidth, windowHeight);
    engine = Engine.create();
    world = engine.world;

    // Create car body
    let carBody = Bodies.rectangle(200, height - 100, 80, 40, { friction: 0.5, density: 0.02 });
    let wheel1 = Bodies.circle(170, height - 80, 20, { restitution: 0.5 });
    let wheel2 = Bodies.circle(230, height - 80, 20, { restitution: 0.5 });

    car = Matter.Composite.create();
    Matter.Composite.add(car, [carBody, wheel1, wheel2]);
    World.add(world, car);

    // Create ground
    ground = Bodies.rectangle(width / 2, height - 10, width, 20, { isStatic: true });
    World.add(world, ground);

    // Add obstacles
    for (let i = 1; i <= 5; i++) {
        let obs = Bodies.rectangle(i * 300, height - 50, 100, 20, { isStatic: true });
        obstacles.push(obs);
        World.add(world, obs);
    }
}

function draw() {
    background(135, 206, 235);
    Engine.update(engine);

    drawBody(car.bodies[0]); // Car Body
    drawBody(car.bodies[1]); // Wheel 1
    drawBody(car.bodies[2]); // Wheel 2
    drawBody(ground); // Ground

    obstacles.forEach(drawBody);
}

function drawBody(body) {
    fill(255, 0, 0);
    beginShape();
    body.vertices.forEach(v => vertex(v.x, v.y));
    endShape(CLOSE);
}

function keyPressed() {
    if (keyCode === RIGHT_ARROW) {
        Matter.Body.applyForce(car.bodies[0], car.bodies[0].position, { x: 0.02, y: 0 });
    }
    if (keyCode === LEFT_ARROW) {
        Matter.Body.applyForce(car.bodies[0], car.bodies[0].position, { x: -0.02, y: 0 });
    }
}
    

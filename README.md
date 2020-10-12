# SlingShotGame-MatterJS

//first you can download matterjs libray from npm or clone it from github 
// then embed it into any html page 

let engine = Matter.Engine.create();
let render = Matter.Render.create({
  element: document.body,
  engine:engine
}); 

let ground = Matter.Bodies.rectangle(400,600,810,60,{ isStatic: true}); 
let boxA = Matter.Bodies.rectangle(400,200,80,80);
let boxB = Matter.Bodies.rectangle(450,50,80,80);

Matter.World.add(engine.world,[boxA,boxB,ground,mouseCoonstraint]);
Matter.Engine.run(engine);
Matter.Render.run(render);

let mouse = Matter.Mouse.create(render.canvas);
let mouseConstraint = Matter.MouseConstraint.create(engine, {
  mouse: mouse,
  constraint: {
    render: {visible: false}
  }
});
render.mouse = mouse;
Matter.World.add(engine.world,[boxA,boxB,ground,mouseConstraint]);

let stack = Matter.Composites.stack(200,200,4,4,0,0, function(x,y){
  return Matter.Bodies.rectangle(x,y,80,80);
});

let platform = Matter.Bodies.rectangle(1200, 500, 300, 20, { isStatic: true });
let stack = Matter.Composites.stack(1100, 270, 4, 4, 0, 0, function(x, y) {
    return Matter.Bodies.polygon(x, y, 8, 30); 
});

let ball = Matter.Bodies.circle(300, 600,20);
let sling = Matter.Constraint.create({ 
      pointA: { x: 300, y: 600 }, 
      bodyB: ball, 
      stiffness: 0.05
});


let firing = false;
Matter.Events.on(mouseConstraint,'enddrag', function(e) {
  if(e.body === ball) firing = true;
});


Matter.Events.on(engine,'afterUpdate', function() {
  if (firing && Math.abs(ball.position.x-300) < 20 && Math.abs(ball.position.y-600) < 20) {
      ball = Matter.Bodies.circle(300, 600, 20);
      Matter.World.add(engine.world, ball);
      sling.bodyB = ball;
      firing = false;
  }
});

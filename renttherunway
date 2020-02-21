<html>
  <head>
    <script src="https://cdn.jsdelivr.net/npm/p5@0.10.2/lib/p5.js"></script>
    <script>
    	var loaded = false;
var BLUE, RED;
var tris = [];

function preload(){
	var script = document.createElement( 'script' );
  	script.src = 'https://cdn.rawgit.com/ironwallaby/delaunay/master/delaunay.js';
  	script.onload = function(){
    	loaded = true;
      	initializeTriangulation();
    }
    document.body.appendChild( script );
}

function setup(){
    createCanvas( windowWidth, windowHeight );
    
    BLUE = color( '#1E2630' );
    RED = color( '#B22538' ); //color( '#FB3550' );7F1A28
    
    initializeTriangulation();

    t0 = new Timeline('RENT THE RUNWAY', 'subTitle', 'Background', 'What Ever ..... bla bla', windowWidth/2, 1);
    t1 = new Timeline('Background', 'sutdy', '2008\nmarch', 'kjhfdjsfhf body1', windowWidth/2+200, 2);
  	t2 = new Timeline('Get Started', '_subTitle', '2020\nmay', 'dhgjfhgjhgureiefije body2', windowWidth/2+400, 3);
}

function initializeTriangulation(){
  	if( !loaded ) return;
  	frameCount = 0;
    tris = [];
    var pts = [];
    // push canvas rect points
    pts.push( createVector( 0, 0 ) );
    pts.push( createVector( width, 0 ) );
    pts.push( createVector( width, height ) );
    pts.push( createVector( 0, height ) );
    
    // add a certain nb of pts proportionally to the size of the canvas
    // ~~ truncates a floating point number and keeps the integer part, like floor()
    var n = ~~ ( width / 300 * height / 300 );
    for( var i = 0; i < n; i ++ ){
        pts.push( createVector( ~~ random( width ), ~~ random( height ) ) );
    }
    
    // Now, let's use Delaunay.js
    // Delaunay.triangulate expect a list of vertices (which should be a bunch of two-element arrays, representing 2D Euclidean points)
    // and it will return you a giant array, arranged in triplets, representing triangles by indices into the passed array
    // Array.map function let us create an Array of 2 elements arrays [ [x,y],[x,y],..] from our array of PVector [ PVector(x,y), PVector(x,y), ... ]
    var triangulation = Delaunay.triangulate( pts.map( function( pt ){
        return [ pt.x, pt.y ];
    } ) );
    
    // create Triangles object using indices returned by Delaunay.triangulate
    for( var i = 0; i < triangulation.length; i += 3 ){
        tris.push( new Triangle(
            pts[ triangulation[ i ] ],
            pts[ triangulation[ i + 1 ] ],
            pts[ triangulation[ i + 2 ] ]
        ) );
    }
}

// class for keeping triangles from 3 PVectors
function Triangle( _a, _b, _c ){
    // PVectors
    this.a = _a;
    this.b = _b;
    this.c = _c;
    
    // used for fill using lerpColor
    this.r = random(0.8);
    
    // used for drawing lines on triangles
    // number of lines to draw proportionnally to the triangle size
    this.n = ~~( dist( 
        _a.x, _a.y,
        ( this.b.x + this.c.x ) / 2, ( this.b.y + this.c.y ) / 2
    ) / random( 25, 50 ) ) + 1 ;
    // direction point for the lines
    this.drawTo = ~~ random( 3 );
    
    this.draw = function(){
        noStroke();
        fill( lerpColor( RED, BLUE, this.r ) );
        
        triangle( this.a.x, this.a.y, this.b.x, this.b.y, this.c.x, this.c.y );
        
        switch( this.drawTo ){
            case 0:
                this.drawLines( this.a, this.b, this.c );
                break;
            case 1:
                this.drawLines( this.c, this.a, this.b );
                break;
            case 2:
                this.drawLines( this.b, this.a, this.c );
                break;
        }
        
        stroke( BLUE );
        strokeJoin( BEVEL );
        strokeWeight( 15 );
        noFill();
        triangle( this.a.x, this.a.y, this.b.x, this.b.y, this.c.x, this.c.y );
    };
    
    this.drawLines = function( from, to1, to2 ){
        var c = cos( frameCount / 360 * TWO_PI ) / 2;
        
        for( var i = 1; i <= this.n ; i++ ){
            var p1 = createVector( 
                lerp( from.x, to1.x, ( i - 1 ) / this.n ), 
                lerp( from.y, to1.y, ( i - 1 ) / this.n )
            );
            var p2 = createVector(
                lerp( from.x, to2.x, ( i - 1 ) / this.n ),
                lerp( from.y, to2.y, ( i - 1 ) / this.n )
            );
            var p3 = createVector(
                lerp( from.x, to2.x, ( i - 0.5 + c ) / this.n ),
                lerp( from.y, to2.y, ( i - 0.5 + c ) / this.n )
            );
            var p4 = createVector( 
                lerp( from.x, to1.x, ( i - 0.5 + c ) / this.n ), 
                lerp( from.y, to1.y, ( i - 0.5 + c ) / this.n )
            );
            
            // line( p1.x, p1.y, p2.x, p2.y );
            
            noStroke();
            fill( BLUE );
            quad( p1.x, p1.y, p2.x, p2.y, p3.x, p3.y, p4.x, p4.y );
        }
    }
}

function draw(){
    background( RED );
    
    tris.forEach( t => t.draw() );
    
    if( frameCount % 360 == 0 ) initializeTriangulation();

    //fill('#9FC2CC');
    //background(0,180);
    //fill(220);
 //    textFont('Playfair Display');
	// textSize(100);
	// textAlign(CENTER, CENTER);
	// text('RENT THE RUNWAY', windowWidth/2, windowHeight/2  - windowHeight/3);
	// head();
	//background(0,100);

	t0.display();
	t1.display();
  	t2.display();
  	head();
}

// function mousePressed(){
//     initializeTriangulation();
// }

function windowResized(){
    resizeCanvas( windowWidth, windowHeight);
    initializeTriangulation();
    t1.y = windowHeight/2;
    t2.y = windowHeight/2;
}

function head(){
	push();
	let aString = 'RENT THE RUNWAY,  sutdy case,  CSC489 class';
	let sWidth = textWidth(aString);
	noStroke();
	fill('#EDB183');
    textFont('Arial');
	textSize(14);
	textAlign(LEFT, LEFT);
	text(aString, 20, 20);
	pop();
}







/////Class Timeline
class Timeline {
  constructor(title, subTitle, date, body, x, c) {
    this._title = title;
    this._subTitle = subTitle;
    this._date = date;
    this._body = body;
    this._x = x;
    this._y = windowHeight/2;
    this._s = 80;
    this._exp = false;
    this._dec = 0;
    this._c = c;
  }
  //title
  set title(x) {
    this._title = x;
  }
  get title() {
    return this._title;
  }
  //subTitle
  set subTitle(x) {
    this._subTitle = x;
  }
  get subTitle() {
    return this._subTitle;
  }
  //date
  set date(x) {
    this._date = x;
  }
  get date() {
    return this._date;
  }
  //body
  set body(x) {
    this._body = x;
  }
  get body() {
    return this._body;
  }
  //x, y
  set x(x) {
    this._x = x;
  }
  get x() {
    return this._x;
  }
  set y(x) {
    this._x = x;
  }
  get y() {
    return this._y;
  }

  //functions
  display() {
    var d = dist(this._x, this._y, mouseX, mouseY);
    
    if (this._exp) {
      if (this._s < 140)
        this._s += 2;
      if (this._dec <= 255)
        this._dec += 4;
      push();
      stroke(30, 38, 48, this._dec);
      strokeJoin(BEVEL);
      strokeWeight(10);
      fill(255, this._dec);
      textAlign(CENTER);
      textFont('Playfair Display');
      textSize(100);
      text(this._title, windowWidth/2, windowHeight/5);
      textSize(70);
      text(this._subTitle, windowWidth/2, windowHeight/3);
      textSize(40);
      text(this._body, windowWidth/2, windowHeight/(1.4));
      fill(30, 38, 48);
      stroke(255, this._dec);
      ellipse(this._x, this._y, this._s, this._s);
      fill(255);
      noStroke();
      textSize(this._s / 6);
      text(this._date, this._x, this._y);
      pop();
    } else {
      if (this._s > 100)
        this._s -= 2;
      if (this._dec >= 0)
        this._dec -= 4;
      push();
      stroke(30, 38, 48, this._dec);
      strokeJoin(BEVEL);
      strokeWeight(10);
      fill(255, this._dec);
      textAlign(CENTER);
      textFont('Playfair Display');
      textSize(100);
      text(this._title, windowWidth/2, windowHeight/5);
      textSize(70);
      text(this._subTitle, windowWidth/2, windowHeight/3);
      textSize(40);
      text(this._body, windowWidth/2, windowHeight/(1.4));
      fill(30, 38, 48);
      stroke(255, this._dec);
      ellipse(this._x, this._y, this._s, this._s);
      fill(255);
      noStroke();
      textSize(this._s / 6);
      text(this._c+'/27', this._x, this._y);
      pop();
    }
    if (mouseIsPressed && d < 40)
      this._exp = true;
    if (mouseIsPressed && d > 60 && mouseY > this._y - 25 && mouseY < this._y + 25)
      this._exp = false;
    if (mouseIsPressed && mouseY > this._y - 25 && mouseY < this._y + 25)
      this._x += mouseX - pmouseX;
  }
}





    </script>
    <style type="text/css">
    	html, body {
		  height: 100%;
		}
		body {
		  margin: 0;
		  display: flex;

		  /* This centers our sketch horizontally. */
		  justify-content: flex-end;

		  /* This centers our sketch vertically. */
		  align-items: flex-start;
		}
    </style>
  </head>
  <body>
  </body>
</html>

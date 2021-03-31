const canvas = document.getElementById("canvas");
const context = canvas.getContext("2d");
const width = canvas.width = 350;
const height = canvas.height = 480;
const frameWidth = 248;
const frameHeight = 248;
const xPos = 55;
const yPos = 110;
const scale = 1;
const fps = 120;
const secondsToUpdate = 0.016 * fps;
let frameIndex = 0;
let count = 0;

canvas.style.marginTop = window.innerHeight / 2 -height / 2 + "px";

const spriteAnimate = new Image();
    spriteAnimate.src = "SpriteAnimation/Assets/Images/Flip7.png";

const State = {
    states: {},
    generateState: function (name, startIndex, endIndex){
        if (!this.states[name]){
            this.states[name] = {
                frameIndex: startIndex,
                startIndex: startIndex,
                endIndex: endIndex
            }; 
        }
    },
    getState: function(name) {
        if (this.states[name]){
            return this.states[name];
        };
    }
};

State.generateState("heads", 0, 50);
State.generateState("tails", 0, 50);

function animate(state){
    context.drawImage(
        spriteAnimate,
        0,
        state.frameIndex * frameHeight,
        frameWidth,
        frameHeight,
        xPos,
        yPos,
        frameWidth * scale,
        frameHeight * scale
    );

    count ++;
    if (count > secondsToUpdate){
        state.frameIndex ++;
        count = 0;
    }
    if (state.frameIndex > state.endIndex){
        state.frameIndex = state.startIndex;
    }
}
function frame(){
    context.clearRect(0,0, width, height);
    animate(State.getState("tails"));
    requestAnimationFrame(frame);
}

frame();
<html>

    <head>
        <link href="" rel="stylesheet" type= "text/css">
        <style>
            *, *::before, *::after {
              box-sizing: border-box;
              font-family: cursivesans-serif;
              font-weight: normal;
            }

            body {
                padding: 0;
                margin: 0;
                background: linear-gradient(to right, #00AAFF, #00FF6C);
            }

            .calculator {
                display: grid;
                justify-content: center;
                align-content: center;
                min-height: 100vh;
                grid-template-columns: repeat(4, 100px);
                grid-template-rows: minmax(120px, auto) repeat(5, 100px);
            }

            .calculator button {
                cursor: pointer;
                font-size: 2rem;
                border: 1px solid white;
                outline: none;
                background-color: rgba(255, 255, 255, .75);
            }

            .calculator button:hover {
                background-color: rgba(255, 255, 255, .9);
            }

            .span-two{
                grid-column: span 2
            }

            .output{
                grid-column: 1 / -1;
                background-color: rgba(0,0,0,.75);
                display: flex;
                align-items: flex-end;
                justify-content: space-around;
                flex-direction: column;
                padding: 10px;
                word-wrap: break-word;
                word-break: break-all;
            }

            .output .previous-operand{
                color: rgba(255,255,255,.75);
                font-size: 1.5rem;
            }

            .output .current-operand{
                color: rgba(255,255,255,1);
                font-size: 2.5rem;
            }
        </style>
    </head>
    <body>  
         <div class="calculator">
            <div class="output">
                <div data-previous-operand class="previous-operand">
                </div>
                <div data-current-operand class="current-operand">
                </div>
                </div> 
                <button data-all-clear class="span-two">AC</button>
                <button data-delete>DEL</button>
                <button data-operation>%</button>
                <button data-number>1</button>
                <button data-number>2</button>
                <button data-number>3</button>
                <button data-operation>*</button>
                <button data-number>4</button>
                <button data-number>5</button>
                <button data-number>6</button>
                <button data-operation>+</button>
                <button data-number>7</button>
                <button data-number>8</button>
                <button data-number>9</button>
                <button data-operation>-</button>
                <button data-number>.</button>
                <button data-number>0</button>
                <button data-equals class="span-two">=</button>
        </div>
        
        <script>
            class Calculator{
                constructor(previousOperandTextElement, currentOperandTextElement) {
                    this.previousOperandTextElement = previousOperandTextElement
                    this.currentOperandTextElement = currentOperandTextElement
                    this.clear()
                }
                clear() {
                    this.currentOperand = ''
                    this.previousOperand = ''
                    this.operation = undefined
                }
                
                delete() {
                    this.currentOperand = this.currentOperand.toString().slice(0, -1)
                }
                
                appendNumber(number){
                    if(number=="."&&this.currentOperand.includes("."))
                        return;
                    this.currentOperand = this.currentOperand.toString()+number.toString();
                }
                
                chooseOperation(operation){
                    if(this.currentOperand=="") 
                        return;
                    if(this.currentOperand!=""){
                        this.compute();
                    }
                    this.operation = operation;
                    this.previousOperand = this.currentOperand +" "+this.operation+" ";
                    this.currentOperand = "";
                }
                
                compute(){
                    let computation;
                    const prev = parseFloat(this.previousOperand);
                    const current = parseFloat(this.currentOperand);
                    if(isNaN(prev)||isNaN(current)) return;
                    switch(this.operation){
                        case "+":
                            computation = prev+current;
                            break;
                        case "-":
                            computation = prev-current;
                            break;
                        case "*":
                            computation = prev*current;
                            break;
                        case "%":
                            computation = prev/current;
                            break;
                        default:
                            return;
                    }
                    this.currentOperand=computation;
                    this.operation=undefined;
                    this.previousOperand ="";
                }
                
                updateDisplay(){
                    this.currentOperandTextElement.innerText = this.currentOperand;
                    this.previousOperandTextElement.innerText = this.previousOperand;
                }
            }
            const numberButtons = document.querySelectorAll("[data-number]");
            const operationButtons = document.querySelectorAll("[data-operation]");
            const equalsButtons = document.querySelector("[data-equals]");
            const deleteButtons = document.querySelector("[data-delete]");
            const allClearButtons = document.querySelector("[data-all-clear]");
            const previousOperandTextElement = document.querySelector("[data-previous-operand]");
            const currentOperandTextElement = document.querySelector("[data-current-operand]");
            
            const calculator = new Calculator(previousOperandTextElement,currentOperandTextElement);
            numberButtons.forEach(button=>{
                button.addEventListener("click",() =>{
                    calculator.appendNumber(button.innerText)
                    calculator.updateDisplay();
                })
            });
            
            operationButtons.forEach(button=>{
                button.addEventListener("click",() =>{
                    calculator.chooseOperation(button.innerText);
                    calculator.updateDisplay();
                })
            });
            
            equalsButtons.addEventListener("click", button =>{
                calculator.compute();
                calculator.updateDisplay();
            })
            
            allClearButtons.addEventListener("click", button =>{
                calculator.clear();
                calculator.updateDisplay();
            });
            
            deleteButtons.addEventListener("click", button =>{
                calculator.delete();
                calculator.updateDisplay();
            })
        </script>
    </body>
</html>

<!--
Ten Things I leanred
1. function addEventListener, which will trigger a function as the button experience certain event, such as click
2. function querySelector/querySelectAll, one will select one object with the tag indicated, the other will find all objects with the tag
3. How to record the number clicked, simply read the text of the button and add it to the number we had
4. How to set up CSS and HTML body for a calculator, using grids and buttons mainly
5. How to use Swich statement to do calculation, set up each case of button clicked
6. Operation "=>", acts as a function that only be used one time
7. CSS grid-template-columns, limit column of the grid
8. CSS grid-template-rows, limit rule of the grid
9. CSS Function Minmax() limit the value to a set min and max value
10. Setting a nice background color using color gradient-->

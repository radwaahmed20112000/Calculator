<template>

    <div class="result">{{ firstNumber + " " + operand + " " + secondNumber }}</div>
    <button class="btn" @click="operateSignleX('percent')"><b>%</b></button>
    <button class="btn" id="btn1" @click="reset()"><b>C</b></button>
    <button class="btn" @click="delet()"><i class="material-icons">backspace</i></button>
    <button class="btn" @click="operateSignleX('oneX')"><b>¹/x</b></button>
    <button class="btn" @click="operateSignleX('square')"><b>x²</b></button>
    <button class="btn" @click="operateSignleX('root')"><b>√x</b></button>
    <button class="btn" @click="getOperand('÷')"><b>÷</b></button>
    <button class="btn" @click="getNumber('7')"><b>7</b></button>
    <button class="btn" @click="getNumber('8')"><b>8</b></button>
    <button class="btn" @click="getNumber('9')"><b>9</b></button>
    <button class="btn" @click="getOperand('x')"><b>x</b></button>
    <button class="btn" @click="getNumber('4')"><b>4</b></button>
    <button class="btn" @click="getNumber('5')"><b>5</b></button>
    <button class="btn" @click="getNumber('6')"><b>6</b></button>
    <button class="btn" @click="getOperand('-')"><b>-</b></button>
    <button class="btn" @click="getNumber('1')"><b>1</b></button>
    <button class="btn" @click="getNumber('2')"><b>2</b></button>
    <button class="btn" @click="getNumber('3')"><b>3</b></button>
    <button class="btn" @click="getOperand('+')"><b>+</b></button>
    <button class="btn" @click="operateSignleX('minus')">+/-</button>
    <button class="btn" @click="getNumber('0')"><b>0</b></button>
    <button class="btn" @click="getNumber('.')"><b>.</b></button>
    <button class="btn" @click="getOperand('=')"><b>=</b></button>
</template>

<script>
import axios from 'axios';
export default {    
    name: 'App',
    data() {
        return {
            operand: '',
            firstNumber: '',
            secondNumber: '',
            first: false,
            result: '',
            operatorDone: false,
            operation:'',
            operationDone:false
        }
    },
    methods: {
        getNumber(number) {
            if(this.operationDone ){
                this.reset();
                this.operationDone = false;
            }
            if (!this.first) {
                this.firstNumber += number;
            } else {
                this.secondNumber += number;
            }

        },
        delet() {
            if (!this.first) {
                this.firstNumber = this.firstNumber.slice(0, -1);
            } else {
                this.secondNumber = this.secondNumber.slice(0, -1);
            }
        },
        operateSignleX(operation) {
            this.operation = operation;
            if(this.first){
                this.sendData(this.secondNumber,this.firstNumber,false);
                this.checkRootNegative(this.secondNumber);
                this.operationDone = true
            }else{
                this.sendData(this.firstNumber,this.secondNumber,true);
                this.checkRootNegative(this.firstNumber);
                this.operationDone = true
            }
            
        },
        checkRootNegative(number){
            if(parseFloat(number) < 0){
                alert("You Can't Take Root to a Negative Number!");
                this.reset();
            }
        },
        Operation() {
            if(this.operand == "+"){this.operation =  "add";}
            else if(this.operand == "x"){this.operation = "multiply";}
            else if(this.operand == "-"){this.operation = "subtract";}
            else if(this.operand == "%"){this.operation = "percent";}
            else if(this.operand == "÷"){this.operation = "divide";}
        },
        sendData(first,second,bool){
            //posting Data in APi
            console.log(first+ second+ this.operation);
            axios.get(
                `http://localhost:8085/result`,
                {
                    params:{
                        firstNumber:first,
                        secondNumber:second,
                        operator:this.operation
                }
            })
            .then(Response =>{
                if(bool){
                    this.firstNumber = Response.data;
                }
                else{
                    this.secondNumber = Response.data;
                }
                this.checkError(Response.data);
            });
        },
        getOperand(operand) {
            this.operationDone = false;
            if(this.firstNumber == ''){
                return;
            }
            if(operand != "="){
                this.operand = operand;
            }
            if (!this.operatorDone) {
                this.first = true;
                this.operatorDone = true;
            } else {
                this.Operation();
                this.sendData(this.firstNumber,this.secondNumber,true);
                this.secondNumber = '';
                if(operand == "="){
                    var temp = this.firstNumber;
                    this.reset();
                    this.firstNumber = temp;
                    this.operationDone = true
                }

            }
        },
        reset() {
            this.operand = '',
                this.firstNumber = '',
                this.secondNumber = '',
                this.first = false,
                this.result = '',
                this.operatorDone = false
        },
        checkError(data){
            if (data == "E"){
                this.reset();
                alert("Can't Divide By Zero!");
            }
        },
    
    },

}
</script>

<style>
button {
    border: none;
    color: black;
    background-color: #ADEBE8;
    padding: 0px;
    font-size: 16px;
    margin: 5px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    transition: .5s ease;
    text-align: center;
    border-radius: 50%;

}
button:hover{
    color: white;
    background-color: #6baca9;
}
.result {
    border: solid;
    background-color: #effcfb;
    border-width: 0.5px 0.5px;
    padding: 14px;
    margin-top: 5px;
    margin-bottom: 5px;
    width: 230px;
    height: 20px;
}

#btn1 {
    width: 100px;
}

div {
    border: solid;
    border-width: 0.5px;
    background-color: rgb(6, 70, 64);
    height: 425px;
    width: 260px;
    padding: 5px;

}
</style>

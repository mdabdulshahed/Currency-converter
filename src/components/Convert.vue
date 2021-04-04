<template>
    <div>
        <center>
        <label id='selectl' for='select'>Select any currency  </label>
        <select id='select' v-model="val">
            <option value='inr'>INR</option>
            <option value='usd'>USD</option>
            <option value='eur'>EUR</option>
            <option value='gbp'>GBP</option>         
        </select>

        <input id='d1' type="text" v-model="val1"> 
        <br><br><br>


        <label id='d2l' for='d2'></label>
        <input id='d2' type="text" v-model="val2"><br>
        <label id='d3l' for='d3'></label>
        <input id='d3' type="text" v-model="val3"><br>
        <label id='d4l' for='d4'></label>
        <input id='d4' type="text" v-model="val4">
        </center>
    </div>
</template>

<script>
export default {
    data() {
        return {
            val1: 0,
            val2: 0,
            val3: 0,
            val4: 0,
            val: 0,
            results: {
                GBP: 0.850834,
                INR: 85.926298,
                USD: 1.174722
            },
            curr : 0
        }
    },
    mounted() {
        fetch('http://data.fixer.io/api/latest?access_key=2441f0ef021326b8f6427b8888d0c3dd&symbols=INR,USD,GBP')
        .then(res => res.json())
        .then(data => this.results = data.rates)
        .then(data => console.log(data));
    },
    name : 'value',
    methods: {
        
    },
    watch: {
        val1: function (e) {
            const digits_only = string => [...string].every(c => '0123456789'.includes(c));
            if(! digits_only(e)){
                alert('Please enter a valid Number')    
            }
            if(this.curr == 'eur'){
                this.val2 = (e * this.results.INR).toFixed(3)
                this.val3 = (e * this.results.USD).toFixed(3)
                this.val4 = (e * this.results.GBP).toFixed(3)
            }
            if(this.curr == 'inr'){
                this.val2 = (e * this.results.USD / this.results.INR).toFixed(3)
                this.val3 = (e / this.results.INR).toFixed(3)
                this.val4 = (e * this.results.GBP / this.results.INR).toFixed(3)
            }
            if(this.curr == 'usd'){
                this.val2 = (e * this.results.INR / this.results.USD).toFixed(3)
                this.val3 = (e * this.results.INR / 100).toFixed(3)
                this.val4 = (e * this.results.GBP / this.results.USD).toFixed(3)
            }
            if(this.curr == 'gbp'){
                this.val2 = (e * this.results.INR * this.results.USD).toFixed(3)
                this.val3 = (e * this.results.USD / this.results.GBP).toFixed(3)
                this.val4 = (e * this.results.INR / this.results.GBP * this.results.USD / 100).toFixed(3)
            }
        },
        val: function(event) {
            this.val1 = 0
            this.val2 = 0
            this.val3 = 0
            this.val4 = 0
            switch (event) {
                case 'inr':
                    this.curr = 'inr';
                    document.getElementById('d2l').innerHTML = 'USD'
                    document.getElementById('d3l').innerHTML = 'EUR'
                    document.getElementById('d4l').innerHTML = 'GBP'
                    break;
                case 'usd': 
                    this.curr = 'usd';
                    document.getElementById('d2l').innerHTML = 'INR'
                    document.getElementById('d3l').innerHTML = 'EUR'
                    document.getElementById('d4l').innerHTML = 'GBP'
                
                break;
                case 'eur': 
                    this.curr = 'eur';
                    document.getElementById('d2l').innerHTML = 'INR'
                    document.getElementById('d3l').innerHTML = 'USD'
                    document.getElementById('d4l').innerHTML = 'GBP'
                    break;
                case 'gbp': 
                    this.curr = 'gbp';
                    document.getElementById('d2l').innerHTML = 'INR'
                    document.getElementById('d3l').innerHTML = 'USD'
                    document.getElementById('d4l').innerHTML = 'EUR'                    
                    break;
                default:
                    break;
            }
        }
    }
}
</script>


<style>
    *{
        background-color: aqua;
    }
    div{
        margin-top: 200px;
    }    
    #selectl{
        font-size: 20px;
        font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
    }
    #select{
        font-size: 20px;
    }
    #d1{
        margin: 10px;
        font-size: 20px;
        width: 200px;
        text-align: center;
    }
    input{
        margin: 20px;
        font-size: 20px;
        width: 200px;
        text-align: center;
        border-color: rgb(1, 84, 180);
        border: 2px solid rgb(1, 84, 180);
        border-radius: 5px;
    }
    input:hover, input:focus{
        border: 2px solid rgb(0, 197, 138);
        border-radius: 5px;
    }


</style>
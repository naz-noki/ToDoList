<template>
  <div class="wrapper" id="app">
        <div class="title"><h1>Список заметок</h1></div>
        <div class="pole">
            <input 
            type="text" class="inp" placeholder="Введите значение заметки" 
            v-model="inputValue"
            @keydown.enter="addNewNote"
            >
            <button class="btn" @click="addNewNote">ДОБАВИТЬ</button>  
        </div>
        <div class="itog" >
            <ul v-if="notes.length!==0" >
                <li 
                    v-for="(note, idx) in notes" 
                    :key="note">
                        <input type="checkbox" class="checkbox" v-model="note.check">
                        <p 
                            class="keklol" :class="{ textCheck: note.check }" 
                            v-if="note.forRedakt == false" 
                            >{{note.title}}</p>

                        <i class="gg-chevron-up-r" @click="note.forRedakt = true" v-if="note.forRedakt == false"></i>
                        <i class="gg-close-r" @click="notDisplay(idx)" v-if="note.forRedakt == false"></i>

                        <div class="redakt"  v-if="note.forRedakt == true">
                            <input class='red_inp' type="" v-model="inputRedakt" @keydown.enter="redakt(idx)">
                            <i class="gg-check-r" @click="redakt(idx)"></i>
                        </div>                        
                </li>  
            </ul>
            <p v-if="notes.length!==0" class="lolkek">Количество заметок: {{notes.length}}</p>
        </div> 
        <div class="net" v-if="notes.length==0">Заметок пока нет! Добавьте первую!</div>
    </div>
</template>

<script>

export default {
  data(){
        return {
            inputValue: '',
            counter: 1,
            notes: [],
            inputRedakt: '',
            NewCheck: false
        }
    },
    mounted(){
        this.getItem();
        this.toDoCounter = this.notes.length
    },
    methods: {
        addNewNote(){
            if(this.inputValue !== ''){
                const note = {
                    title: this.inputValue,
                    check: false,
                    forRedakt: false
                };
                this.notes.push(note);
                this.inputValue = '';
            }
        },
        notDisplay(idx){
            this.notes.splice(idx, 1);
        },
        redakt(idx){
            if(this.inputRedakt != ''){
                const note = {
                    title: this.inputRedakt,
                    check: false,
                    forRedakt: false
                };
                this.notes.splice(idx, 1, note);
            }
        },
        getItem(){
            const localNotes = localStorage.getItem('notes');
            if (localNotes) {
                this.notes = JSON.parse(localNotes)
            }
        },
    },
    watch: {
        notes: {
            handler(updatelist){
                localStorage.setItem('notes', JSON.stringify(updatelist));
            },
            deep: true
        }
    }
  
}
</script>

<style>
.textCheck{
    text-decoration: line-through;
    color: rgb(175, 175, 175);
    word-break:break-all;
    width: 1335px;    
}
*{
    padding: 0;
    margin: 0;
}
body {
    width: 100%;
    height: 100%;
    background-color: rgb(24, 23, 73);
}
.wrapper{
    margin: 0 auto;
    margin-top: 15px;
    margin-bottom: 15px;
    max-width: 1450px;
    min-height: 250px;
    background-color: #fff;
    border-radius: 15px;
}
.title{
    margin-left: 20px;
    padding-top: 35px;
    color: rgb(20, 20, 110);
}
.pole{
    margin-left: 20px;
    padding-top: 30px;
    max-width: 1400px;
    min-height: max-content;
    padding-bottom: 15px;
    border-bottom: 1px solid rgb(107, 107, 107);
    display: block;
}
.inp{
    width: 1400px;
    height: 30px;
    border-radius: 5px;
    border: 1px solid rgb(107, 107, 107);
}
.btn{
    background-color: #fff;
    border: 2px solid rgb(71, 175, 88);
    padding: 5px;
    cursor: pointer;
    border-radius: 15px;
    color: rgb(71, 175, 88);
    margin-top: 20px;
}.btn:hover{
    background-color: rgb(71, 175, 88);
    color: #fff;
    transition: 555ms;
}
li {
    border-bottom: 1px solid rgb(107, 107, 107);
    width: 1400px;
    list-style-type: none;
    margin-left: 20px;
    padding-top: 5px;
    padding-bottom: 5px;
    display: flex;
    align-items: center;
}
.itog{
    padding-bottom: 3px;
}
.keklol {
    word-break:break-all;
    width: 1335px;
}
.checkbox{
    margin-right: 5px;
}
.lolkek{
    margin-top: 15px;
    margin-left: 20px;
    margin-bottom: 15px;
}
.redakt{
    width: 1400px;
    display: flex;
    align-items: center;
}
.net{
    margin-top: 15px;
    margin-left: 20px;
    margin-bottom: 15px;
    color: rgb(151, 151, 151);    
}
.red_inp{
    border: none;
    border-bottom: 2px solid rgb(151, 151, 151);   
    outline: none;
    width: 1335px;
    height: 25px;
}

/*  */

.gg-chevron-up-r {
    box-sizing: border-box;
    position: relative;
    display: block;
    transform: scale(var(--ggs,1));
    width: 22px;
    height: 22px;
    border: 2px solid;
    border-radius: 4px;
    margin-left: 10px;
    cursor: pointer;
}
.gg-chevron-up-r::after {
    content: "";
    display: block;
    box-sizing: border-box;
    position: absolute;
    width: 6px;
    height: 6px;
    border-top: 2px solid;
    border-right: 2px solid;
    transform: rotate(-45deg);
    left: 6px;
    bottom: 5px
}

/*  */

.gg-close-r {
    cursor: pointer;
    box-sizing: border-box;
    position: relative;
    display: block;
    transform: scale(var(--ggs,1));
    width: 22px;
    height: 22px;
    border: 2px solid;
    border-radius: 4px;
    margin-left: 10px;    
}
.gg-close-r::after,
.gg-close-r::before {
    content: "";
    display: block;
    box-sizing: border-box;
    position: absolute;
    width: 12px;
    height: 2px;
    background: currentColor;
    transform: rotate(45deg);
    border-radius: 5px;
    top: 8px;
    left: 3px
}
.gg-close-r::after {
    transform: rotate(-45deg)
}

/*  */

.gg-check-r {
    cursor: pointer;
    box-sizing: border-box;
    position: relative;
    display: block;
    width: 22px;
    height: 22px;
    border: 2px solid;
    transform: scale(var(--ggs,1));
    border-radius: 4px;
    cursor: pointer;
    margin-left: 10px;
}
.gg-check-r::after {
    content: "";
    display: block;
    box-sizing: border-box;
    position: absolute;
    left: 3px;
    top: -1px;
    width: 6px;
    height: 10px;
    border-width: 0 2px 2px 0;
    border-style: solid;
    transform-origin: bottom left;
    transform: rotate(45deg)
}
</style>

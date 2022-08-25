<template>
<div>

  <h1>Microphone can be used only to pick up sound, 
    but also to project sound similar to a speaker.</h1>
    <input type="radio" name="options" value="true"/>
    <label for="options">True</label>
    <input type="radio" name="options" value="false"/>
    <label for="options">False</label>
    <div>
    <button class="send" type="button">Send</button>
   </div>

</div>
   
</template>
<script>






export default {
    name: 'quiz-vue',

    created() {
        this.axios
        .get('https://opentdb.com/api.php?amount=1&category=18')
        .then((response) => {
            console.log(response.data.results[0])
        })
    }
    

   


}


</script>




<style scoped>
input [type=radio]{
    margin: 12px 4px;
}

.send{
    margin-top: 12px;
    height: 40px;
    max-width: 120px;
    padding: 0 30px;
    color:#fff;
    background-color: #1867c0 ;
    cursor: pointer;

    
}

</style>
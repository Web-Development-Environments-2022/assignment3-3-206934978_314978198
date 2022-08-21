<template>
    <div id="aboutPage">
    
        <div class="container"
        >

        <img
            class="image"
            src="@/assets/YuvalAndNoa.png"
            width="500"
            height="300"
            id="icon"
        />

            <h1 class="title">Our Website</h1>
            <p>
                Hi there,
                Welcome to our Recipes Website!

                Here you can look for recipes, save your special recipes, tagged as a "favorite"
                recipe if you want, anddddd save your family' recipes!
            </p>
            <br>
            <h3> We are Yuval Cohen & Noa Magrisso </h3>
            <br/>
            <br/>

            <h4> If you want you can see here our previous projects: </h4>
            <br/>
            <p>
                1. Building a website for someone in the third generation who close to us,
                <br/>
                so he or she will not feel lonely. Due to Covid-19 it was looked as a good idea.
                <br/>
                A link: <a href="https://github.com/Web-Development-Environments-2022/206934978"> Link to assignment 1 - of Noa </a>
                <br/>
                A link: <a href="https://github.com/Web-Development-Environments-2022/314978198"> Link to assignment 1 - of Yuval </a>
            <br/>
            <br/>
                2. A PacMan game. You know the rules of the game.....
                <br/>
                A link: <a href="https://github.com/Web-Development-Environments-2022/314978198_206934978"> assignment2 </a> <br>
            </p>
        </div>
        <div>
        
        </div>
                
    </div>
</template>

<script>
export default {
  data() {
    return {
        joke: {
            avatar: 'backgroundperfect.png'
        }
    };
  }
};

</script>
    
<style lang="css">
.image {
  margin-left: 300px;
}

p {
    font-size: 20px;
}

</style>
<script setup>
    import jobs from "./data/data.json"
    import './style.css'
    import { ref } from "vue";

    const filter = ref([])

    const getImageUrl = filename =>  import.meta.env.BASE_URL + 'src/assets/' + filename
    const addFilter = job_filter => !filter.value.includes(job_filter) ? filter.value = [...filter.value, job_filter] : null
    const removeFilter = job_filter_remove => filter.value = filter.value.filter(item => item!==job_filter_remove)
    const clear = () =>  filter.value = []

    const allFilter = job => {
        const fullList = [job.role, job.level, ...job.languages]
        const skills = filter.value.filter( item => fullList.includes(item))
        return skills.length == filter.value.length
    }

</script>

<template>
    <div class="container">
        <header>
            <img src="./assets/images/bg-header-desktop.svg" alt="desktop header backgound">
            <div class="filter"  v-if="filter.length >=1 ">
                <ul class="list_filter">
                    <div class="items_filter">
                        <li v-for="item in filter">{{ item }} <button @click="removeFilter(item)"><img src="./assets/images/icon-remove.svg" alt=""></button></li>
                    </div>
                    <button class="clear" @click="clear">Clear</button>
                </ul>
            </div>
        </header>
        <main>
            <div>
                <ul class="jobs">
                    <li class="job_li" v-for="job in jobs" :key="job.id" :hidden="!allFilter(job)">
                        <div class="job">
                            <div class="job_description">
                                <div class="line" :class="{line_green: job.new && job.featured}"></div>
                                <img :src="getImageUrl(job.logo)" alt="">   
                                <div class="all_descriptions">
                                    <div class="li_header">
                                        <span class="company">{{ job.company }}</span>
                                        <span class="new" v-if="job.new">New!</span>
                                        <span class="featured" v-if="job.featured">Featured</span>
                                    </div>
                                    <p>{{ job.position }}</p>
                                    <div class="li_footer">
                                        <span class="posted_at">{{ job.postedAt }}</span>
                                        <span class="contract">{{ job.contract }}</span>
                                        <span class="location">{{ job.location }}</span>
                                    </div>
                                </div>
                            </div>  
                            <div class="job_skills">
                                <span class="role" @click="addFilter(job.role)">{{ job.role }}</span>
                                <span class="level" @click="addFilter(job.level)">{{ job.level }}</span>
                                <span class="languages" v-for="language in job.languages" @click="addFilter(language)">{{ language }}</span>
                            </div>
                        </div>
                       
                    </li>
                </ul>
            </div>
        </main>
        <footer class="attribution">
            Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank">Frontend Mentor</a>. 
            Coded by <a href="#">Your Name Here</a>.
        </footer>
    </div>
</template>

<style scoped>
    .container {
        background-color: var(--light-grayish-cyan-background);
        min-height: 100vh;
        position: relative;
    }
    header {
        position: relative;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }
    header img {
        width: 100%;
        background-color: var(--desaturated-dark-cyan);
    }
    .filter {
        width: 80%;
        max-width: 80em;
        min-width: 45em;
    }
    .list_filter{
        position: absolute;
        display: flex;
        justify-content: space-between;
        bottom: -2em;
        left: 50%;
        transform: translate(-50%,0);
        border-radius: 5px;
        background-color: #FFF;
        list-style-type: none;
        width: 80%;
        max-width: 80em;
        min-width: 45em;
        padding: 0.5em;
    }
    .items_filter{
        display: flex;
    }
    .list_filter li{
        color: var(--desaturated-dark-cyan);
        background-color: var(--light-grayish-cyan-background);
        border-radius: 5px;
        padding: 0.5em;
        margin: 0.5em;
        list-style: none;
    }
    button {
        border: none;
        background: none;
    }
    .clear{
        margin-right: 2em;
        color: var(--dark-grayish-cyan);
    }
    button img {
        height: 100%;
    }
    .jobs {
        list-style-type: none;      
        display: flex;
        flex-direction: column;
        align-items: center; 
        padding: 3em 0;
    }
    .job_li {
        background-color: #FFF;
        border-radius: 10px;
        width: 80%;
        min-width: 45em;
        max-width: 80em;
        margin: 1em 0;
        box-shadow: 0px 10px 20px var(--desaturated-dark-cyan-shadow);
        height: 100%;
    }
    .job{
        display: flex;
        justify-content: space-between;
        align-items: center;  
    }
    .job_description {
        display: flex;
        align-items: center;
        width: 50%;
    }
    .line, .line_green {
        width: 5px;
        height: 10em;
        border-radius: 10px 0 0 10px;
        margin-right: 2em;
    }
    .line_green {
        background-color: var(--desaturated-dark-cyan);
    }
    .job_description img {
        margin-right: 1em;
    }
    .all_descriptions {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        gap: 1.2em;
    }
    .li_header {
        display: flex;
        align-items: center;
        flex-wrap: wrap;
    }
    .company {
        color: var(--desaturated-dark-cyan);
    }
    .new, .featured {
        text-transform: uppercase;
        font-weight: 700;
        color: white;
        padding: 0.5em;
        margin: 0 0.5em;
        border-radius: 20px;
    }
    .new {
        background-color: var(--desaturated-dark-cyan);
    }
    .featured {
        background-color: var(--very-dark-grayish-cyan);
    }
    p {
        font-size: 18px;
        color: var(--very-dark-grayish-cyan);
    }
    .li_footer {
        color: var(--dark-grayish-cyan);
        display: flex;
        width: 100%;
        height: 100%;
        justify-content: center;
        align-items: center;
    }
    .li_footer span{
        display: flex;
        align-items: center;
    }
    .li_footer span::after {
        margin: 0 1em;
        content:'\00B7';
    }
    .li_footer span:last-child::after {
        margin: 0em;
        content:none;
    }
    .job_skills {
        display: flex;
        justify-content: flex-end;
        align-items: center;
        flex-wrap: wrap;
        width: 50%;
        padding-right: 2em;
    }
    .role, .level, .languages{
        color: var(--desaturated-dark-cyan);
        background-color: var(--light-grayish-cyan-filter);
        border-radius: 5px;
        padding: 0.5em;
        margin: 0.5em;
    }
    .attribution { 
        position: absolute;
        bottom: 1em;
        left: 50%;
        transform: translate(-50%, 0);
        font-size: 11px; 
        text-align: center; 
        
    }
    .attribution a { color: hsl(228, 45%, 44%); }

    button img:hover {
        cursor: pointer;
    }
    .clear:hover {
        color: var(--desaturated-dark-cyan);
        text-decoration: underline;
        cursor: pointer;
    }
    p:hover {
        color: var(--desaturated-dark-cyan);
        cursor: pointer;
    }
    .job_skills span:hover{
        cursor: pointer;
        background-color: var(--desaturated-dark-cyan);
        color: #FFF;
    }

</style>
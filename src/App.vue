<script setup>
    import jobs from "./data/data.json"
    import './style.css'
    import { ref } from "vue";

    const filter = ref([])

    const getImageUrl = filename =>  import.meta.env.BASE_URL + 'public/' + filename
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
            <img class="desktop_image_header" src="../public/images/bg-header-desktop.svg" alt="desktop header backgound">
            <img class="mobile_image_header" src="../public/images/bg-header-mobile.svg" alt="mobile header backgound" hidden>
            <div class="filter"  v-if="filter.length >=1 ">
                <ul class="list_filter">
                    <div class="items_filter">
                        <li class="li_items_filter" v-for="item in filter">{{ item }} <button @click="removeFilter(item)"><img src="../public/images/icon-remove.svg" alt="remove filter"></button></li>
                    </div>
                    <button class="clear" @click="clear">Clear</button>
                </ul>
            </div>
        </header>
        <main>
            <div>
                <ul class="jobs">
                    <li class="job_li" v-for="job in jobs" :key="job.id" :hidden="!allFilter(job)" :class="{line_green1: job.new && job.featured}">
                        <div class="job">
                            <div class="job_description line" >
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
                            <hr>
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
            Coded by <a href="https://www.linkedin.com/in/felipe-stefani-a35185116/" target="_blank">Felipe Stefani</a>.
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
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        width: 100%;
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
        display: flex;
        margin-top: -2em;
        justify-content: space-between;
        border-radius: 5px;
        background-color: #FFF;
        list-style-type: none;
        padding: 0.5em;
    }
    .items_filter{
        display: flex;
        flex-wrap: wrap;
    }
    .li_items_filter{
        color: var(--desaturated-dark-cyan);
        background-color: var(--light-grayish-cyan-background);
        border-radius: 5px;
        padding-left: 0.5em;
        margin: 0.5em;
        list-style: none;
        display: flex;
        align-items: center;
        font-weight: 700;
    }
    .li_items_filter img{
        background-color: transparent;
    }
    button {
        border: none;
        background: none;
    }
    .li_items_filter button{
        padding:0.5em;
        border-radius: 0 5px 5px 0;
        margin-left: 0.5em;
        background-color: var(--desaturated-dark-cyan);
        height: 100%;
    }
    .clear{
        margin-right: 2em;
        color: var(--dark-grayish-cyan);
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
        margin-bottom: 1em;
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
        padding: 1em 0;
    }
    .line_green1 {
        height: 100%;
        border-radius: 5px;
        border-left: 6px solid var(--desaturated-dark-cyan);
    }
    .job_description img {
        margin: 0 1em 0 2em;
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
        font-weight: 700;
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

    .li_items_filter button:hover {
        cursor: pointer;
        background-color: var(--very-dark-grayish-cyan);
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

    @media (max-width: 750px) {
        .desktop_image_header{
            display: none;
        }
        .mobile_image_header{
            display: inline-block;
            height: 10em;
        }
        .filter{
            min-width: 0em;
        }
        .jobs{
            margin-top: 1em;
            gap: 2em;
        }
        .list_filter, .job_li{
            min-width: 0;
        }
        .job{
            flex-direction: column;
        }
        .job_description img {
            position: absolute;
            margin: 0;
            padding: 0;
            top: -1.5em;
            left: 1em;
            width: 3em;
        }
        .job_description, .job_skills {
            position: relative;
            width: 100%;
            padding-top: 2em;
            padding-left: 1em;
        }
        .company{
            padding-right: 1em;
        }
        .li_footer{
            justify-content: flex-start;
        }
        hr {
            width: 90%;
            align-self: self-start;
            margin: 0.5em 0 1em 1em;
        }
        .job_skills {
            justify-content: flex-start;
            margin: 0;
            padding: 0 1em 1em 0.5em;
        }
    }

</style>
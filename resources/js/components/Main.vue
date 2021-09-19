<template>
    <div class="container pt-5">
        <div class="row mt-5">
            <div class="col-6 py-2" v-for="post in posts" :key="post.id">
                <div class="card w-100" style="width: 18rem;">
                    <div class="card-body">
                        <h4 class="card-title">{{post.title}}</h4>
                        <p class="card-text">{{post.content}}</p>
                        <a href="#" class="btn btn-primary">Show Details</a>
                    </div>
                </div>
            </div>
            
        </div>
        <nav aria-label="Page navigation example">
            <ul class="pagination">
                <li class="page-item" @click="getPost(currentPage - 1)">
                    <a class="page-link" href="#" aria-label="Previous">
                        <span aria-hidden="true">&laquo;</span>
                    </a>
                </li>
                <li v-for="page in lastPage" :key="page" @click="getPost(page)" class="page-item" :class="(currentPage == page) ? 'active':''" >
                    <a class="page-link" href="#">{{page}}</a>
                </li>

                <li class="page-item" @click="getPost(currentPage + 1)">
                    <a class="page-link" href="#" aria-label="Next">
                        <span aria-hidden="true">&raquo;</span>
                    </a>
                </li>
            </ul>
        </nav>
        
    </div>
</template>
<script>


export default {

        name : "Main",
        data() {
            return {
                apiLink : 'http://127.0.0.1:8000/api/posts',
                posts : [],
                currentPage : 1,
                lastPage : null
            }
            
        },
        created() {
            // gli dico di prendere la pagina 1 
            this.getPost(1);
        },
        methods : {
            getPost(postPage) {
                axios.get(this.apiLink,{
                    params: {
                        page : postPage
                    }
                })
                    .then(rensponse => {
                        // console.log(rensponse.data.results.data[0].id);

                        // con paginate si puo prendere la pagina corrente e l'ultima 
                        this.currentPage = rensponse.data.results.current_page;
                        this.lastPage = rensponse.data.results.last_page;

                        this.posts = rensponse.data.results.data;
                    })
            }
        }

}
</script>

<style lang="scss" scoped>

</style>
<template>
    <div class="row">
        <div class="col-12">
            <div class="card">
                <div class="card-header">
                    <h4>Add Category</h4>
                </div>
                <div class="card-body">
                    <form @submit.prevent="create">
                        <div class="row">
                            <div class="col-12 mb-2">
                                <div class="form-group">
                                    <label>Title</label>
                                    <input type="text" class="form-control" v-model="category.title">
                                </div>
                            </div>
                            <div class="col-12 mb-2">
                                <div class="form-group">
                                    <label>Description</label>
                                    <input type="text" class="form-control" v-model="category.description">
                                </div>
                            </div>
                        <div>
                        <input type="file" class="form-control" @change="onClick">
    
                        </div>
                            
                            <div class="col-10">
                                <button type="submit" class="btn btn-primary">Save</button>
                            </div>
                        </div>                        
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    name:"add-category",
    
    data(){
        return {
            category:{
                title:"",
                description:""
            },
            file:null,
        }
    },

    methods:{
        onClick(e) {
                this.file = e.target.files[0];
            },
        async create(e){
            const config = {
                    headers: {
                        'content-type': 'multipart/form-data'
                    }
                }
            let data = new FormData();
            // console.log(this.category.files);
            data.append('file',this.file);
            data.append('title',this.category.title);
            data.append('description',this.category.description);
          
            await this.axios.post('/api/category',data,config).then(response=>{
                this.$router.push({name:"categoryList"})
            }).catch(error=>{
                console.log(error)
            })
        }
    }
}
</script>
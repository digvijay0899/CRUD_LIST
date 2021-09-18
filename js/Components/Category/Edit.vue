<template>
  <div class="row">
    <div class="col-12">
      <div class="card">
        <div class="card-header">
          <h4>Update Category</h4>
        </div>
        <div class="card-body">
          <form @submit.prevent="update">
            <div class="row">
              <div class="col-12 mb-2">
                <div class="form-group">
                  <label>Title</label>
                  <input
                    type="text"
                    class="form-control"
                    v-model="category.title"
                  />
                </div>
              </div>
              <div class="col-12 mb-2">
                <div class="form-group">
                  <label>Description</label>
                  <input
                    type="text"
                    class="form-control"
                    v-model="category.description"
                  />
                </div>
                <div>
                  <label>Upload File</label>
                  <input type="file" class="form-control" @change="onClick"/>
                </div>
              </div>
              <div class="col-12">
                <button type="submit"  class="btn btn-primary">Update</button>
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
  name: "update-category",
  data() {
    return {
      category: {
        title: "",
        description: "",
        _method: "post",
      },
      file:null,
    };
  },
  mounted() {
    this.showCategory();
  },
  methods: {
    async showCategory() {
      await this.axios
        .get(`/api/category/${this.$route.params.id}`)
        .then((response) => {
          const { title, description} = response.data;
          this.category.title = title;
          this.category.description = description; 
        })
        .catch((error) => {
          console.log(error);
        });
    },
     onClick(e) 
    {
        console.log('onclick');
        this.file = e.target.files[0];
        //console.log(e.target.files[0]);
    },
    async update() {
      if (confirm("Are you sure you want to update?")) {
              const config1 = {
                    headers: {
                        'content-type': 'multipart/form-data'
                    }
                }
          let data = new FormData();
            // console.log(this.category.files);
            //console.log('file:'+JSON.stringify(this.file));
            data.append('file',this.file);
            data.append('title',this.category.title);
            data.append('description',this.category.description);
        await this.axios
          .post(`/api/category/${this.$route.params.id}`, data, this.category, config1)
          
          .then((response) => {
            this.$router.push({ name: "categoryList" });
          })
          .catch((error) => {
            console.log(error);
          });
      }
    },
  },
};
</script>
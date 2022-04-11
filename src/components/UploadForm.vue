<template>
    <span id="msg" class=" form-control alert" ></span>

    <form @submit.prevent="uploadPhoto" method="POST" id="uploadForm" >
        <div class="form-group">
            <label for="desc">Description</label>
            <textarea class="form-control" id="desc" rows="3" name="description" ></textarea>
        </div>

        <div class="form-group">
            <label for="photo">Photo Upload</label><br>
            <input type="file" class="form-control-file" id="photo"  name="photo" >
        </div>

        <input type="submit" class="btn btn-primary"/>
         
    </form>

</template>

<script>
export default {
    data() {
        return {
            csrf_token: ''
        }        
    },

    methods: {
        uploadPhoto() {
            let uploadForm = document.getElementById('uploadForm');
            let form_data = new FormData(uploadForm);

            fetch("/api/upload", {
                method: 'POST',
                body: form_data,
                headers: {
                    'X-CSRFToken': this.csrf_token
                }
            })
                .then(function (response) {
                    return response.json();
                })
                .then(function (data) {
                    // display a success message
                    console.log(data);
                })
                .catch(function (error) {
                    console.log(error);
                });
        },

        getCsrfToken() {
            let self = this;
            
            fetch('/api/csrf-token')
                .then((response) => response.json())
                .then((data) => {
                    console.log(data);
                    self.csrf_token = data.csrf_token;
                })
        }
    },

    created() {
        this.getCsrfToken();
    },
}
</script>
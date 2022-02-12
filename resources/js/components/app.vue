<template>
    <div class="mt-4">
        <file-pond
            name="image"
            ref="pond"
            label-idle="Click to choose image or drag here..."
            @init="filepondInitialized"
            accepted-file-types="image/*"
        />
    </div>
</template>
<script>
import vueFilePond, { setOptions } from 'vue-filepond';
import "filepond/dist/filepond.min.css";
import FilePondPluginFileValidationType from 'filepond-plugin-file-validate-type';

setOptions({
    server: {
        process: {
            url: './upload',
            headers: {
                'X-CSRF-TOKEN': document.head.querySelector('meta[name="csrf_token"]').content
            }
        }
    }

})

const FilePond = vueFilePond(FilePondPluginFileValidationType);

export default {
    components: {
        FilePond
    },
    data() {
        return {
            images: []
        }
    },
    mounted(){
        axios.get('/images')
            .then((response) => {
                this.images = response.data;
            })
            .catch((error) => {
                console.error(error);
            });
    },
    methods: {
        filepondInitialized() {
            console.log('Filepond is ready!');
            console.log('Filepond object: ', this.$refs.pond);
        }
    }
}
</script>
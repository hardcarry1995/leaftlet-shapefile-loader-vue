<template>
    <div>
        <div id="map"></div>
        <div class='uploader'>
            <label for="input">Select a zipped shapefile:</label> <input type="file" ref='fileInput'> <br>
            <input type="submit" id="submit" @click='handleSubmit'> <span id="warning"></span>
        </div>
    </div>
</template>
  
<script>
export default {
    data() {
        return {
            map: null
        }
    },
    methods: {
        handleSubmit() {
            const files = this.$refs.fileInput.files;
            if (files.length == 0) {
                return; //do nothing if no file given yet
            }

            const file = files[0];

            if (file.name.slice(-3) != 'zip') { //Demo only tested for .zip. All others, return.
                alert('Select .zip file');
                return;
            } else {
                this.handleZipFile(file);
            }
        },
        handleZipFile(file) {
            const reader = new FileReader();
            reader.onload = () => {
                if (reader.readyState != 2 || reader.error) {
                    return;
                } else {
                    this.convertToLayer(reader.result);
                }
            }
            reader.readAsArrayBuffer(file);
        },
        convertToLayer(buffer) {
            shp(buffer).then((geojson) => {
                const layer = L.shapefile(geojson).addTo(this.map);
            });
        }
    },
    mounted() {
        this.map = L.map('map', {
            center: [7.2, 40.9],
            zoom: 2
        });

        L.tileLayer('http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(this.map);
    }
}
</script>

<style>
#map {
    height: 100vh;
}
.uploader {
    position: absolute;
    bottom: 0;
    left: 0;
    background-color: white;
    z-index: 1000;
}
</style>
<template>
    <div id="app" class="container">
        <div class="page-header">
            <h1><img src="http://dulichchauagiare.com/logo-du-lich-chau-a-gia-re.png" alt="" width="100" height="60"><span>Du Lịch</span></h1>
        </div>
        <div class="row">
            <div class="panel panel-default col-md-4">
                <div class="panel-heading">
                    <h3 class="panel-title">Thêm Điểm Du Lịch Mới</h3>
                </div>
                <div class="panel-body">
                    <form id="form" class="form" v-on:submit.prevent="addTour">
                        <div class="form-group row col-md-12 text-center">
                            <img :src="preview" width="150" height="150" />
                        </div>
                    <div class="form-group row col-md-12">
                            <label for="hinh anh">Hình Ảnh:</label>
                            <input type="file" class="123" ref="imageFile" @change="changeImage" required>
                        </div>
                        <div class="form-group row col-md-12">
                            <label for="madiemdulich">Mã Điểm Du Lịch:</label>
                            <input type="text" id="madiemdulich" class="form-control" v-model="newTour.ma" required>
                        </div>
                        <div class="form-group row col-md-12">
                            <label for="tendiemdulich">Tên Điểm Du Lịch:</label>
                            <input type="text" id="tendiemdulich" class="form-control" v-model="newTour.ten" required>
                        </div>
                        <div class="form-group row col-md-12">
                            <label for="diadiem">Địa Điểm:</label>
                            <input type="text" id="diadiem" class="form-control" v-model="newTour.diadiem" required>
                        </div>
                        <div class="form-group row col-md-12">
                            <label for="bookUrl">Giới thiệu:</label>
                            <input type="text" id="gioithieu" class="form-control" v-model="newTour.gioithieu" required>
                        </div>
                        <div class="form-group row col-md-12">
                            <label for="bookUrl">Rating:</label>
                            <input type="number" id="rating" min="0" max="5" class="form-control" v-model="newTour.rating" required>
                        </div>
                        <div class="form-group row col-md-12">
                            <input type="submit" class="btn btn-primary" value="Thêm Mới">
                        </div>
                    </form>
                </div>
            </div>
            <div class="panel panel-default col-md-8">
                <div class="panel-heading">
                    <h3 class="panel-title">Danh sách</h3>
                </div>
                <div class="panel-body">
                    <table class="table table-striped">
                        <thead>
                            <tr>
                                <th>Mã Điểm Du Lịch</th>
                                <th>Tên</th>
                                <th>Hình Ảnh</th>
                                <th>Địa Điểm</th>
                                <th>Giới Thiệu</th>
                                <th>Rating</th>
                            </tr>
                        </thead>
                        <tbody 
                            name="custom-classes-transition"
                            enter-active-class="animated tada"
                            leave-active-class="animated bounceOutRight"
                            is="transition-group"
                        >
                            <tr 
                                v-for="tour in tours" 
                                v-bind:key="tour['.key']"
                            >
                                <td>{{ tour.ma }}</td>
                                <td>{{ tour.ten }}</td>
                                <td> 
                                    <img :src="tour.hinhanh" width="100" height="100" />
                                </td>
                                <td>{{ tour.diadiem }}</td>
                                <td>{{ tour.gioithieu }}</td>
                                <td>{{ tour.rating }}</td>
                                <td><span class="glyphicon glyphicon-trash" aria-hidden="true" v-on:click="remove(tour)"></span></td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import Firebase from 'firebase'
    import toastr from 'toastr'

    let config = {
        apiKey: "AIzaSyAyW1tqSHnlv0kdCR0r4711yImSvhb9lWc",
        authDomain: "dulich-4282e.firebaseapp.com",
        databaseURL: "https://dulich-4282e.firebaseio.com",
        projectId: "dulich-4282e",
        storageBucket: "dulich-4282e.appspot.com",
        messagingSenderId: "421241029703"
    };
      
    let app = Firebase.initializeApp(config)
    let db = app.database()
    let toursRef = db.ref('tours')
    var storage = app.storage('gs://dulich-4282e.appspot.com')
    let upLoad = storage.ref()
    
    export default {
        name: 'app',
        data () {
            return {
                newTour: {
                    ma: '',
                    ten: '',
                    diadiem: '',
                    hinhanh: '',
                    gioithieu: '',
                    rating: '',
                },
                valueImage: '',
                preview: 'http://qh-hdnd.phuyen.gov.vn/wps/wcm/connect/c9aed34d-4f57-42de-9e6e-cbfde85bb5e5/photo_default_0.png?MOD=AJPERES&CACHEID=c9aed34d-4f57-42de-9e6e-cbfde85bb5e5',
                uploadTask: '',
                errors: []
            }
        },
        firebase: {
            tours: toursRef,
        },
        methods: {
            changeImage(e) {
                e.preventDefault();
                let reader = new FileReader();
                let file = e.target.files[0];

                reader.onloadend = () => {
                    this.newTour.hinhanh = file
                    this.preview = reader.result
                }

                this.uploadTask = upLoad.child(`images/${file.name}`).put(file);
                this.uploadTask.then(snapshot => {
                    this.newTour.hinhanh = this.uploadTask.snapshot.downloadURL;
                })
                reader.readAsDataURL(file)
                console.log(this.checkImage)
            },

            addTour(e) {
                this.errors = [''];
                if(!this.newTour.ma) {
                    this.errors.push("Ma dia diem required.")
                    toastr.warning("Ma dia diem required.");
                }

                if(!this.newTour.ten) {
                    this.errors.push("Ten dia diem required.")
                    toastr.warning("Ten dia diem required.");
                }
              
                if(!this.newTour.diadiem) {
                    this.errors.push("Dia diem required.")
                    toastr.warning("Dia diem required.");
                }
               
                if(!this.newTour.ten) {
                    this.errors.push("Gioi thieu required.")
                    toastr.warning("Gioi thieu required.");
                }
              
                if(!this.newTour.rating) {
                    this.errors.push("Rating required.")
                    toastr.warning("Rating required.");
                }
               
                e.preventDefault();
                if(this.errors == '') {
                    toursRef.push(this.newTour);
                    this.$refs.imageFile.value = '';
                    this.newTour.ma = '';
                    this.newTour.ten = '';
                    this.newTour.diadiem = '';
                    this.newTour.hinhanh = '';
                    this.newTour.gioithieu = '';
                    this.newTour.rating = '';
                    this.preview = 'http://qh-hdnd.phuyen.gov.vn/wps/wcm/connect/c9aed34d-4f57-42de-9e6e-cbfde85bb5e5/photo_default_0.png?MOD=AJPERES&CACHEID=c9aed34d-4f57-42de-9e6e-cbfde85bb5e5',

                    toastr.success('Tour added successfully')
                }
            },

            remove(tour) {
                toursRef.child(tour['.key']).remove()
                toastr.success('Tour removed successfully')
            }
        },
    }
</script>
<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: #2c3e50;
        margin-top: 20px;
        width: 1800px;
    }

    ul li {
        list-style-type: none;
    }

    .text-center {
        text-align: center
    }
</style>

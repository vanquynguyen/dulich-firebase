<template>
    <div id="app" class="container">
        <div class="page-header">
            <h1><img src="http://dulichchauagiare.com/logo-du-lich-chau-a-gia-re.png" alt="" width="100" height="60"><span>Du Lịch</span></h1>
        </div>
        <div class="row">
            <ul class="nav nav-tabs" role="tablist">
                <li role="presentation" class="active"><a href="#diem-dl" aria-controls="diem-dl" role="tab" data-toggle="tab" aria-expanded="true">Điểm du lịch</a></li>
                <li role="presentation" class=""><a href="#danhgia" aria-controls="danhgia" role="tab" data-toggle="tab" aria-expanded="false">Đánh giá</a></li>
                <li role="presentation" class=""><a href="#user" aria-controls="user" role="tab" data-toggle="tab" aria-expanded="false">Người dùng</a></li>
                <li role="presentation" class=""><a href="#comment" aria-controls="comment" role="tab" data-toggle="tab" aria-expanded="false">Statitics</a></li>
            </ul>
            <div class="tab-content">
                <div role="tabpanel" class="tab-pane active" id="diem-dl">
                    <div class="row">
                        <div class="panel panel-default col-md-4">
                            <div class="panel-heading">
                                <h3 class="panel-title">Thêm Điểm Du Lịch Mới</h3>
                            </div>
                            <div class="panel-body">
                                <form 
                                    id="form" 
                                    class="form" 
                                    v-on:submit.prevent="addTour"
                                >
                                <div class="form-group row col-md-12">
                                        <label 
                                            for="hinh anh"
                                        >
                                            Hình Ảnh:
                                        </label>
                                        <input 
                                            type="file" 
                                            class="123" ref="imageFile" 
                                            @change="changeImage" 
                                            required
                                            multiple
                                        />
                                    </div>
                                    <div class="form-group row col-md-12">
                                        <label 
                                            for="madiemdulich"
                                        >
                                            Mã Điểm Du Lịch:
                                        </label>
                                        <input 
                                            type="text" 
                                            id="madiemdulich" 
                                            class="form-control" 
                                            v-model="newTour.ma" 
                                            required
                                        />
                                    </div>
                                    <div class="form-group row col-md-12">
                                        <label 
                                            for="tendiemdulich"
                                        >
                                            Tên Điểm Du Lịch:
                                        </label>
                                        <input 
                                            type="text" 
                                            id="tendiemdulich" 
                                            class="form-control" 
                                            v-model="newTour.ten" 
                                            required
                                        />
                                    </div>
                                    <div class="form-group row col-md-12">
                                        <label 
                                            for="diadiem"
                                        >
                                            Địa Điểm:
                                        </label>
                                        <input 
                                            type="text" 
                                            id="diadiem" 
                                            class="form-control" 
                                            v-model="newTour.diadiem" 
                                            required
                                        />
                                    </div>
                                    <div class="form-group row col-md-12">
                                        <label 
                                            for="gioithieu"
                                        >
                                            Giới thiệu:
                                        </label>
                                        <input 
                                            type="text" 
                                            id="gioithieu" 
                                            class="form-control" 
                                            v-model="newTour.gioithieu" 
                                            required
                                        />
                                    </div>
                                    <div class="form-group row col-md-12">
                                        <label 
                                            for="rating"
                                        >
                                            Rating:
                                        </label>
                                        <input 
                                            type="number" 
                                            id="rating" 
                                            min="0" 
                                            max="5" 
                                            class="form-control" 
                                            v-model="newTour.rating" 
                                            required
                                        />
                                    </div>
                                    <div class="form-group row col-md-12">
                                        <input 
                                            type="submit" 
                                            class="btn btn-primary" 
                                            value="Thêm Mới"
                                        />
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
                                                <button type="button" @click="showImage(tour.hinhanh)" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal">
                                                    Chi tiết
                                                </button>
                                            </td>
                                            <td>{{ tour.diadiem }}</td>
                                            <td>{{ tour.gioithieu }}</td>
                                            <td>{{ tour.rating }}</td>
                                            <td><span class="glyphicon glyphicon-trash" aria-hidden="true" v-on:click="removeTour(tour)"></span></td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                            <!-- Modal -->
                            <div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
                                <div class="modal-dialog" role="document">
                                    <div class="modal-content">
                                        <div class="modal-header">
                                            <h5 class="modal-title" id="exampleModalLabel">Các hình ảnh</h5>
                                            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                                            <span aria-hidden="true">&times;</span>
                                            </button>
                                        </div>
                                        <div class="modal-body">
                                            <div class="image-detail row">
                                                <div 
                                                    class="col-sm-4"
                                                    v-for="image in showImages" 
                                                    v-bind:key="image['.key']"
                                                >
                                                    <img :src="image" width="150" class="img-responsive"/>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div role="tabpanel" class="tab-pane" id="danhgia">
                     <div class="row">
                        <div class="panel panel-default col-md-4">
                            <div class="panel-heading">
                                <h3 class="panel-title">Thêm Đánh Giá Mới</h3>
                            </div>
                            <div class="panel-body">
                                <form 
                                    id="form" 
                                    class="form" 
                                    v-on:submit.prevent="addRate"
                                >
                                    <div class="form-group row col-md-12">
                                        <label 
                                            for="madiemdulich"
                                        >
                                            Mã Điểm Du Lịch:
                                        </label>
                                        <select  
                                            class="form-control" 
                                            v-model="newRate.ma" 
                                            required
                                        >
                                            <option v-for="tour in tours" :value="tour.ma" :key="tour.id">{{ tour.ten }}</option>
                                        </select>
                                    </div>
                                    <div class="form-group row col-md-12">
                                        <label 
                                            for="username"
                                        >
                                            UserName:
                                        </label>
                                        <input 
                                            type="text" 
                                            id="username" 
                                            class="form-control" 
                                            v-model="newRate.username" 
                                            required
                                        />
                                    </div>
                                       <div class="form-group row col-md-12">
                                        <label 
                                            for="comment"
                                        >
                                            Bình Luận:
                                        </label>
                                        <input 
                                            type="text" 
                                            id="comment" 
                                            class="form-control" 
                                            v-model="newRate.comment" 
                                            required
                                        />
                                    </div>
                                     <div class="form-group row col-md-12">
                                        <label 
                                            for="rating"
                                        >
                                            Rating:
                                        </label>
                                        <input 
                                            type="number" 
                                            id="rating" 
                                            min="0" 
                                            max="5" 
                                            class="form-control" 
                                            v-model="newRate.rating" 
                                            required
                                        />
                                    </div>
                                    <div class="form-group row col-md-12">
                                        <input 
                                            type="submit" 
                                            class="btn btn-primary" 
                                            value="Thêm Mới"
                                        />
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
                                            <th>Username</th>
                                            <th>Bình luận</th>
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
                                            v-for="rate in rates" 
                                            v-bind:key="rate['.key']"
                                        >
                                            <td>{{ rate.ma }}</td>
                                            <td>{{ rate.username }}</td>
                                            <td>{{ rate.comment }}</td>
                                            <td>{{ rate.rating }}</td>
                                            <td><span class="glyphicon glyphicon-trash" aria-hidden="true" v-on:click="removeRate(rate)"></span></td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>
                <div role="tabpanel" class="tab-pane" id="user">
                
                </div>
                <div role="tabpanel" class="tab-pane" id="comment" style="">
                
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
    let toursRef = db.ref('Diemdulich')
    var storage = app.storage('gs://dulich-4282e.appspot.com')
    let upLoad = storage.ref()
    let ratesRef = db.ref('Danhgia');

    export default {
        name: 'app',
        data () {
            return {
                newTour: {
                    ma: '',
                    ten: '',
                    diadiem: '',
                    hinhanh: [],
                    gioithieu: '',
                    rating: '',
                },
                newRate: {
                    ma: '',
                    usename: '',
                    comment: '',
                    rating: '',
                },
                uploadTask: [],
                showImages: [],
                errors: []
            }
        },
        firebase: {
            tours: toursRef,
            rates: ratesRef
        },
        methods: {
            changeImage(e) {
                e.preventDefault();
                let reader = new FileReader();
                let files = e.target.files;
                const uploadTasks = this.uploadTask;
                for (let i = 0; i < files.length; i++) {
                    let uploadFirebase = upLoad.child(`images/${files[i].name}`).put(files[i]);
                    uploadTasks.push(uploadFirebase);
                }

                for (let i = 0; i < uploadTasks.length; i++) {
                    uploadTasks[i].then(snapshot => {
                        let image =  uploadTasks[i].snapshot.downloadURL
                        this.newTour.hinhanh.push(image);
                    })
                }
            },

            showImage(image) {
                this.showImages = image;
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

            removeTour(tour) {
                toursRef.child(tour['.key']).remove()
                toastr.success('Tour removed successfully')
            },

            addRate(e) {
                this.errors = [''];
                if(!this.newRate.ma) {
                    this.errors.push("Ma dia diem required.")
                    toastr.warning("Ma dia diem required.");
                }

                if(!this.newRate.username) {
                    this.errors.push("Ten nguoi dung required.")
                    toastr.warning("Ten nguoi dung required.");
                }
              
                if(!this.newRate.comment) {
                    this.errors.push("Binh luan required.")
                    toastr.warning("Binh luan required.");
                }
              
                if(!this.newRate.rating) {
                    this.errors.push("Rating required.")
                    toastr.warning("Rating required.");
                }
               
                e.preventDefault();
                if(this.errors == '') {
                    ratesRef.push(this.newRate);
                    this.newRate.ma = '';
                    this.newRate.username = '';
                    this.newRate.comment = '';
                    this.newRate.rating = '';

                    toastr.success('Rate added successfully')
                }
            },

            removeRate(rate) {
                ratesRef.child(rate['.key']).remove()
                toastr.success('Rate removed successfully')
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

    .image-detail {
        margin-left: 4px;
    }
</style>

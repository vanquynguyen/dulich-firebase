<template>
    <div id="app" class="container">
        <div v-if="typeof checkAdmin == 'undefined' || checkAdmin != 'ok'" class="login">
            <div class="container">
                <div class="card card-container">
                    <img id="profile-img" class="profile-img-card" src="//ssl.gstatic.com/accounts/ui/avatar_2x.png" />
                    <p id="profile-name" class="profile-name-card"></p>
                    <form class="form-signin" @submit.prevent="login">
                        <span id="reauth-email" class="reauth-email"></span>
                        <input type="email" id="inputEmail" class="form-control" v-model="email" placeholder="Email address" required autofocus>
                        <input type="password" id="inputPassword" class="form-control" v-model="password" placeholder="Password" required>
                        <div id="remember" class="checkbox">
                            <label>
                                <input type="checkbox" value="remember-me"> Remember me
                            </label>
                        </div>
                        <button class="btn btn-lg btn-primary btn-block btn-signin" type="submit">Sign in</button>
                    </form><!-- /form -->
                    <a href="#" class="forgot-password">
                        Forgot the password?
                    </a>
                </div><!-- /card-container -->
            </div><!-- /container -->
        </div>
        <div v-else>
            <div class="page-header">
                <h1><img src="http://dulichchauagiare.com/logo-du-lich-chau-a-gia-re.png" alt="" width="100" height="60"><span>Du Lịch</span></h1>
            </div>
            <div class="dropdown profile">
                <button class="btn btn-default dropdown-toggle" type="button" id="menu1" data-toggle="dropdown">Admin
                <span class="caret"></span></button>
                <ul class="dropdown-menu" role="menu" aria-labelledby="menu1">
                    <li role="presentation"><a role="menuitem" tabindex="-1" href="#">Profile</a></li>
                    <li role="presentation"><a role="menuitem" tabindex="-1" href="#">Inbox</a></li>
                    <li role="presentation"><a role="menuitem" tabindex="-1" href="#">Change Password</a></li>
                    <li role="presentation" class="divider"></li>
                    <li role="presentation"><a role="menuitem" tabindex="-1" href="#" @click='logout'>Logout</a></li>
                </ul>
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
                        <div class="row content">
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
                                                for="cumdiemdulich"
                                            >
                                                Cụm Điểm Du Lịch:
                                            </label>
                                            <select class="form-control"  
                                                @change="onChangeTour"
                                                v-model="danhmuc" 
                                                id="cumdiemdulich" 
                                                required
                                            >
                                                <option value="" disabled>Chọn cụm du lịch</option>
                                                <option value="mienbac">Miền Bắc</option>
                                                <option value="mientrung">Miền Trung</option>
                                                <option value="miennam">Miền Nam</option>
                                            </select>
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
                                                readonly
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
                        <!-- <div class="panel panel-default col-md-4">
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
                        </div> -->
                        <div class="panel panel-default">
                            <div class="panel-heading">
                                <h3 class="panel-title">Danh sách</h3>
                            </div>
                            <div class="panel-body">
                                <table class="table table-striped">
                                    <thead>
                                        <tr>
                                            <th>Mã Điểm Du Lịch</th>
                                            <th>Tên Điểm Du Lịch</th>
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
                                            <td>{{ rate.tendiemdulich }}</td>
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
                    <div role="tabpanel" class="tab-pane" id="user">
                        <div class="panel panel-default">
                            <div class="panel-heading">
                                <h3 class="panel-title">Danh sách người dùng</h3>
                            </div>
                            <div class="panel-body">
                                <table class="table table-striped">
                                    <thead>
                                        <tr>
                                            <th>FullName</th>
                                            <th>Email</th>
                                        </tr>
                                    </thead>
                                    <tbody 
                                        name="custom-classes-transition"
                                        enter-active-class="animated tada"
                                        leave-active-class="animated bounceOutRight"
                                        is="transition-group"
                                    >
                                        <tr 
                                            v-for="user in users" 
                                            v-bind:key="user['.key']"
                                        >
                                            <td>{{ user.fullName }}</td>
                                            <td>{{ user.email }}</td>
                                            <td><span class="glyphicon glyphicon-trash" aria-hidden="true" v-on:click="removeUser(user)"></span></td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                    <div role="tabpanel" class="tab-pane" id="comment" style="">
                    
                    </div>
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
    let ratesRef = db.ref('Danhgia')
    let usersRef = db.ref('users')

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
                errors: [],
                checkAdmin: '',
                email: '',
                password: '',
                danhmuc: '',
            }
        },

        firebase: {
            tours: toursRef,
            rates: ratesRef,
            users: usersRef
        },

        mounted() {
            let admin = localStorage.getItem('checkAdmin');

            this.checkAdmin = admin;
        },

        methods: {
            login() {
                let email = this.email;
                let password = this.password;

                if (email == 'admin@gmail.com' && password == '12345678') {
                    localStorage.setItem('checkAdmin', 'ok')
                    window.location = '';
                }

                this.errors = [''];
                if(!this.email) {
                    this.errors.push("Email required.")
                    toastr.warning("Email required.");
                }

                if(this.email != 'admin@gmail.com') {
                    this.errors.push("Email Invalid.")
                    toastr.warning("Email Invalid.");
                }

                if(!this.password) {
                    this.errors.push("Password required.")
                    toastr.warning("Password required.");
                }

                if(this.password != '12345678') {
                    this.errors.push("Password Invalid.")
                    toastr.warning("Password Invalid.");
                }
            },

            logout() {
                localStorage.setItem('checkAdmin', '')
                window.location = '';
            },

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

            onChangeTour() {
                const danhmuc = this.danhmuc
                const random = Math.floor(100000 + Math.random() * 900000);
                if (danhmuc == 'mienbac') {
                    this.newTour.ma = `MB${random}`
                }

                if (danhmuc == 'mientrung') {
                    this.newTour.ma = `MT${random}`
                }

                if (danhmuc == 'miennam') {
                    this.newTour.ma = `MN${random}`
                }
            },

            addTour(e) {
                this.errors = [''];
                if(!this.newTour.danhmuc) {
                    this.errors.push("Cum diem du lich required.")
                    toastr.warning("Ten dia diem required.");
                }

                if(!this.newTour.ma) {
                    this.errors.push("Ma diem du lich required.")
                    toastr.warning("Ma dia diem required.");
                }

                if(!this.newTour.ten) {
                    this.errors.push("Ten diem du lich required.")
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
                    this.newTour.danhmuc = '';
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
            },

            removeUser(user) {
                usersRef.child(user['.key']).remove()
                toastr.success('User removed successfully')
            },
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

    .content {
        margin: 0;
    }

    .col-md-4, .col-md-8 {
        padding: 0;
    }

    .text-center {
        text-align: center
    }

    .image-detail {
        margin-left: 4px;
    }

    .profile {
        float: right;
    }

    .profile ul {
        float: left;
        margin-left: -77px;
    }
    /*
    * Specific styles of signin component
    */
    /*
    * General styles
    */
    .login {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: #2c3e50;
        margin-top: 20px;
        height: 100%;
        /* background-repeat: no-repeat;
        background-image: linear-gradient(rgb(104, 145, 162), rgb(12, 97, 33)); */
    }
    .card-container.card {
        max-width: 350px;
        padding: 40px 40px;
    }

    .btn {
        font-weight: 700;
        height: 36px;
        -moz-user-select: none;
        -webkit-user-select: none;
        user-select: none;
        cursor: default;
    }

    /*
    * Card component
    */
    .card {
        background-color: #F7F7F7;
        /* just in case there no content*/
        padding: 20px 25px 30px;
        margin: 0 auto 25px;
        margin-top: 50px;
        /* shadows and rounded borders */
        -moz-border-radius: 2px;
        -webkit-border-radius: 2px;
        border-radius: 2px;
        -moz-box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
        -webkit-box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
        box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
    }

    .profile-img-card {
        width: 96px;
        height: 96px;
        margin: 0 auto 10px;
        display: block;
        -moz-border-radius: 50%;
        -webkit-border-radius: 50%;
        border-radius: 50%;
    }

    /*
    * Form styles
    */
    .profile-name-card {
        font-size: 16px;
        font-weight: bold;
        text-align: center;
        margin: 10px 0 0;
        min-height: 1em;
    }

    .reauth-email {
        display: block;
        color: #404040;
        line-height: 2;
        margin-bottom: 10px;
        font-size: 14px;
        text-align: center;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        -moz-box-sizing: border-box;
        -webkit-box-sizing: border-box;
        box-sizing: border-box;
    }

    .form-signin #inputEmail,
    .form-signin #inputPassword {
        direction: ltr;
        height: 44px;
        font-size: 16px;
    }

    .form-signin input[type=email],
    .form-signin input[type=password],
    .form-signin input[type=text],
    .form-signin button {
        width: 100%;
        display: block;
        margin-bottom: 10px;
        z-index: 1;
        position: relative;
        -moz-box-sizing: border-box;
        -webkit-box-sizing: border-box;
        box-sizing: border-box;
    }

    .form-signin .form-control:focus {
        border-color: rgb(104, 145, 162);
        outline: 0;
        -webkit-box-shadow: inset 0 1px 1px rgba(0,0,0,.075),0 0 8px rgb(104, 145, 162);
        box-shadow: inset 0 1px 1px rgba(0,0,0,.075),0 0 8px rgb(104, 145, 162);
    }

    .btn.btn-signin {
        /*background-color: #4d90fe; */
        background-color: rgb(104, 145, 162);
        /* background-color: linear-gradient(rgb(104, 145, 162), rgb(12, 97, 33));*/
        padding: 0px;
        font-weight: 700;
        font-size: 14px;
        height: 36px;
        -moz-border-radius: 3px;
        -webkit-border-radius: 3px;
        border-radius: 3px;
        border: none;
        -o-transition: all 0.218s;
        -moz-transition: all 0.218s;
        -webkit-transition: all 0.218s;
        transition: all 0.218s;
    }

    .btn.btn-signin:hover,
    .btn.btn-signin:active,
    .btn.btn-signin:focus {
        background-color: rgb(12, 97, 33);
    }

    .forgot-password {
        color: rgb(104, 145, 162);
    }

    .forgot-password:hover,
    .forgot-password:active,
    .forgot-password:focus{
        color: rgb(12, 97, 33);
    }
</style>

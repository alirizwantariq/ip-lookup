<template>
    <div>
        <section class="bg-light p-5">
            <div class="container mt-5 ">
                <div class="d-sm-flex">
                    <div class="heading">
                        <h1 class="text-sm-center text-md-start ">Welcome to <span class="text-warning">Ip Lookup</span>
                            Website.</h1>
                        <p class="lead text-md-start">Search any Public Ip and get it's GeoLocation</p>
                        <!-- LogIn Form -->
                        <div class="mt-5">
                            <div class="pe-5">
                                <h1 class="mb-5 text-success">LogIn To Access the Site,</h1>
                                <div>
                                    <div class="input-group mb-3">
                                        <span class="input-group-text">Email</span>
                                        <input class="form-control" type="email" placeholder="atuny0" id="email"
                                            required />
                                    </div>
                                    <div class="input-group mb-3">
                                        <span class="input-group-text">Password</span>
                                        <input class="form-control" type="password" placeholder="9uQFF1Lh" id="password"
                                            required />
                                    </div>
                                    <button class="btn btn-lg bg-success rounded border text-white mt-3"
                                        type="submit" @click="enter">Log in
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                    <img class="w-50 img-fluid d-none d-sm-block rounded-1 " src="../assets/undraw_Sign_in_re_o58h.png"
                        alt="">
                </div>
            </div>
        </section>
    </div>
</template>

<script>
export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name: "LogIn",
    data() {
        return {
        };
    },
    methods:{
        enter(){
            return this.$router.push({ path: "./Ip-lookup" });
        }
    }
};
</script>
<style scoped>

</style>

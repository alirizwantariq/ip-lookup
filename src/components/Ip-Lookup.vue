<template>
    <div class="container-fluid h-100">
        <section>
            <b-navbar class="bbb" toggleable>
                <b-navbar-brand href="#" class="text-danger">IP Lookup</b-navbar-brand>
                <b-navbar-toggle target="navbar-toggle-collapse">
                    <template #default="{ expanded }">
                        <b-icon v-if="expanded" icon="chevron-bar-up"></b-icon>
                        <b-icon v-else icon="chevron-bar-down"></b-icon>
                    </template>
                </b-navbar-toggle>
                <b-collapse id="navbar-toggle-collapse" is-nav>
                    <b-navbar-nav class="ml-auto">
                        <b-nav-item href="#">Account</b-nav-item>
                        <b-nav-item href="#"><span class="text-danger">Logout</span></b-nav-item>
                    </b-navbar-nav>
                </b-collapse>
            </b-navbar>
        </section>
        <h3 class="text-secondary mt-4">Search any Public IP to get it's GeoLocation 🗺</h3>
        <div class="main d-flex justify-content-between">
            <section class="w-25 my-3">
                <b-input-group>
                    <b-form-input id="input" type="search" v-model="ip" @change="geoLocation"
                        placeholder="Enter IP"></b-form-input>
                    <b-input-group-append>
                        <b-button variant="light" squared @click="geoLocation">Search<b-spinner v-show="!loading" small
                                type="grow"></b-spinner></b-button>
                    </b-input-group-append>
                </b-input-group>
                <div class="mt-5" v-if="token">
                    <b-list-group>
                        <b-list-group-item variant="light">IPv4: {{ query }}</b-list-group-item>
                        <b-list-group-item variant="light">IPv6: {{ ipv6 }} </b-list-group-item>
                        <b-list-group-item variant="light">Status: {{ status }}</b-list-group-item>
                        <b-list-group-item variant="light">Countary {{ country }}</b-list-group-item>
                        <b-list-group-item variant="light">City: {{ city }}</b-list-group-item>
                        <b-list-group-item variant="light">Latitude: {{ lat }}</b-list-group-item>
                        <b-list-group-item variant="light">Longitude: {{ lon }}</b-list-group-item>
                        <b-list-group-item variant="light">Timezone: {{ timezone }}</b-list-group-item>
                        <b-list-group-item variant="light">Isp: {{ isp }}</b-list-group-item>
                    </b-list-group>
                </div>
            </section>
            <section>
                <div class="d-flex mx-5 align-items-center justify-content-center my-3" v-if="token">
                    <h5 class="my-auto">{{ query }} Complete Ip info <a target="_blank"
                            :href="`https://whatismyipaddress.com/ip/${this.query}`">here</a>.</h5>
                </div>
            </section>
            <section class="w-50 my-3">
                <div v-if="token">
                    <b-button variant="light" class="mx-auto" diabled>Location</b-button>
                    <div class="w-auto rounded-lg">
                        <iframe class="w-100 mt-5" height="350" frameborder="0" scrolling="no" marginheight="0"
                            marginwidth="0" v-bind:src="this.src"></iframe>
                    </div>
                </div>
            </section>
        </div>
        <hr class="w-50 mx-auto">
        <br>
        <div>
            <h2>User Comments About {{ query }}</h2>
            <p>No comments.Be the first one to add one</p>
            <br>
            <p>If you have a question contact <a href="#">support</a> or post in the <a href="#">Forums</a>.</p>
            <b-form-textarea class="w-50" rows="5" id="textarea-default"></b-form-textarea>
            <b-button class="mt-2" variant="danger" rows="4" v-model="text" type="submit">Post Comment</b-button>
        </div>
        <hr class="w-50 mx-auto">
        <br>
        <footer class="footer-bs">
            <div class="row">
                <div class="col-md-3 footer-brand animated fadeInLeft">
                    <h2 class=text-danger>Ip Lookup</h2>
                    <p>Suspendisse hendrerit tellus laoreet luctus pharetra. Aliquam porttitor vitae orci nec ultricies.
                        Curabitur vehicula, libero eget faucibus faucibus, purus erat eleifend enim, porta pellentesque
                        ex mi ut
                        sem.</p>
                    <p>© 2022 <span class="text-info">ART</span>, All rights reserved</p>
                </div>
                <div class="col-md-4 footer-nav animated fadeInUp">
                    <h4 class="text-info">Menu —</h4>
                    <div class="col-md-6">
                        <ul class="pages">
                            <li><a href="#">My Ip</a></li>
                            <li><a href="#">Ip Lookup</a></li>
                            <li><a href="#">Hide My Ip</a></li>
                            <li><a href="#">Vpns</a></li>
                            <li><a href="#">Tools</a></li>
                        </ul>
                    </div>
                </div>
                <div class="col-md-2 footer-social animated fadeInDown">
                    <h4 class="text-success">Follow Us</h4>
                    <ul>
                        <li><a href="#">Facebook</a></li>
                        <li><a href="#">Twitter</a></li>
                        <li><a href="#">Instagram</a></li>
                        <li><a href="#">RSS</a></li>
                    </ul>
                </div>
                <div class="col-md-3 footer-ns animated fadeInRight">
                    <h4 class="text-warning">Newsletter</h4>
                    <p>A rover wearing a fuzzy suit doesn’t alarm the real penguins</p>
                    <div class="input-group">
                        <input type="text" class="form-control" placeholder="Enter mail">
                        <span class="input-group-btn">
                            <button class="btn border" type="button">Send</button>
                        </span>
                    </div>
                </div>
            </div>
        </footer>
    </div>
</template>

<script>

export default {
    name: "IpLookup",
    data() {
        return {
            query: "",
            ipv6: "",
            status: "",
            country: "",
            countryCode: "",
            region: "",
            regionName: "",
            city: "",
            zip: "",
            lat: "",
            lon: "",
            timezone: "",
            org: "",
            isp: "",
            token: false,
            src: "",
            loading: true,
            text: '',
            year: '',
        };
    },
    // mounted() {
    //     this.src = `https://maps.google.com/maps?q=${this.lat}, ${this.lon}&z=15&output=embed`;
    // },
    methods: {
        geoLocation() {
            this.token = true;
            if (this.ip == "") {
                alert("Feild cannot be Empty");
            }
            setTimeout(((this.loading = false), 3000));
            fetch(`http://ip-api.com/json/${this.ip}`)
                .then((response) => response.json())
                .then((data) => {
                    console.log(data);
                    this.query = data.query;
                    this.status = data.status;
                    this.country = data.country;
                    this.countryCode = data.countryCode;
                    this.region = data.region;
                    this.city = data.city;
                    this.zip = data.zip;
                    this.lat = data.lat;
                    this.lon = data.lon;
                    this.timezone = data.timezone;
                    this.isp = data.isp;
                    this.org = data.org;
                    this.ipv6 = "Hidden";
                    this.src = `https://maps.google.com/maps?q=${this.lat}, ${this.lon}&z=15&output=embed`;
                    setTimeout(((this.loading = true), 3000));
                });
        },
    },
};
</script>

<style scoped>
.bbb {
    border-bottom: 1px solid black;
}

.footer-bs {
    padding: 60px 0;
    color: black;
}

.footer-bs .footer-brand,
.footer-bs .footer-nav,
.footer-bs .footer-social,
.footer-bs .footer-ns {
    padding: 10px 25px;
}

.footer-bs .footer-nav,
.footer-bs .footer-social,
.footer-bs .footer-ns {
    border-color: transparent;
}

.footer-bs .footer-brand h2 {
    margin: 0px 0px 10px;
}

.footer-bs .footer-brand p {
    font-size: 12px;
}

.footer-bs .footer-nav ul.pages {
    list-style: none;
    padding: 0px;
}

.footer-bs .footer-nav ul.pages li {
    padding: 5px 0px;
}

.footer-bs .footer-nav ul.pages a {
    color: rgba(0, 0, 0, 0.6);
    font-weight: bold;
    text-transform: uppercase;
}

.footer-bs .footer-nav ul.pages a:hover {
    color: rgba(255, 255, 255, 0.80);
    text-decoration: none;
}

.footer-bs .footer-nav h4 {
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 3px;
    margin-bottom: 10px;
}

.footer-bs .footer-nav ul.list {
    list-style: none;
    padding: 0px;
}

.footer-bs .footer-nav ul.list li {
    padding: 5px 0px;
}

.footer-bs .footer-nav ul.list a {
    color: rgba(0, 0, 0, 0.6);
}

.footer-bs .footer-nav ul.list a:hover {
    color: rgba(0, 0, 0, 0.6);
    text-decoration: none;
}

.footer-bs .footer-social ul {
    list-style: none;
    padding: 0px;
}

.footer-bs .footer-social h4 {
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 3px;
}

.footer-bs .footer-social li {
    padding: 5px 4px;
}

.footer-bs .footer-social a {
    color: rgba(0, 0, 0, 0.6);
}

.footer-bs .footer-social a:hover {
    color: rgba(0, 0, 0, 0.6);
    text-decoration: none;
}

.footer-bs .footer-ns h4 {
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 3px;
    margin-bottom: 10px;
}

.footer-bs .footer-ns p {
    font-size: 12px;
    color: rgba(0, 0, 0, 0.6);
}
</style>
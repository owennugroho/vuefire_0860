<template>
    <v-main>
       <v-card elevation="3" class="mt-5 mx-6" style="border-radius: 6px;">
            <v-row justify="center" align="center" style="margin:3px auto;">
                <v-col>
                    <v-text-field v-model="search" class="font-weight-bold" color="black" style="width: 70%;font-family: Poppins; font-size: 20px; font-style:bold; border-radius: 10px;" rounded append-icon="mdi-magnify" outlined placeholder="Search..." hide-details></v-text-field>
                </v-col>
                <v-col>
                    <v-btn class="font-weight-bold" style="margin:10px auto;font-family: Poppins; font-size: 20px; text-transform: capitalize; float:right; color: #F7CACA" x-large color="#93A9D1" @click="dialog = true">New Merchandise</v-btn>
                </v-col>
            </v-row>
        </v-card>
        <v-card elevation="3" style="border-radius: 6px;" class="mt-5 mx-6">
            <v-data-table :headers="headers" :items="merchandises" :search="search" :items-per-page="10">
                <template v-slot:[`item.actions`]="{ item }">
                    <v-icon  color="green darken-2" class="mr-2" @click="editData(item)">mdi-pencil</v-icon>
                    <v-icon  color="red" @click="selectedId = item.id; dialogConfirm = true"> mdi-delete </v-icon>
                </template>
            </v-data-table>
        </v-card>
        <v-dialog transition="dialog-top-transition" max-width="600" v-model="dialog" style="border-radius: 10px;">
            <v-card style="border-radius: 10px;">
                <v-card-title class="pa-0">
                    <v-toolbar color="#93A9D1" elevation="0" style="border-radius: 10px 10px 0px 0px;" height="90%">
                      <span style="color: #F7CACA; font-family: Poppins; font-weight: 800; font-size: 160%; margin-left: 3%;">{{ formTitle }}</span>   
                    </v-toolbar>
                </v-card-title> 
                <v-card-text class="pb-0">
                    <v-container> 
                        <v-form ref="form">
                            <v-text-field outlined color="black" class="textfield mt-3"  v-model="form.merchandise" label="Merchandise Name" required :rules="inputRules"></v-text-field>
                            <v-text-field outlined color="black" class="textfield mt-3"  v-model="form.artist" label="Artist Name" required :rules="inputRules"></v-text-field>
                            <v-text-field outlined color="black" class="textfield"  v-model="form.price" label="Price" :rules="inputRules" prefix="Rp" type="numeric"></v-text-field>
                            <v-text-field outlined color="black" class="textfield"  v-model="form.stock" label="Stock" :rules="inputRules" type="numeric"></v-text-field>
                            <v-text-field outlined color="black" class="textfield"  v-model="form.package" label="Packaging" :rules="inputRules"></v-text-field>
                        </v-form>
                    </v-container> 
                </v-card-text>
                <v-divider></v-divider>
                <v-card-actions class="justify-end">
                    <v-btn color="#EEEEEE" large style="font-family: Poppins; font-size: 20px; text-transform: capitalize; font-weight: 900; color:#001D38;" @click="closeDialog()"> Cancel </v-btn>
                    <v-btn color="#93A9D1" large style="font-family: Poppins; font-size: 20px; text-transform: capitalize; font-weight: 700; color:#F7CACA;" @click="saveData()"> Save </v-btn> 
                </v-card-actions>
            </v-card>
        </v-dialog>
        <v-dialog transition="dialog-top-transition" max-width="400" v-model="dialogConfirm" style="border-radius: 10px;">
            <v-card style="border-radius: 10px;">
                <v-card-title class="pa-0">
                    <v-toolbar color="#EF5350" elevation="0" style="border-radius: 10px 10px 0px 0px;" height="80%">
                      <span style="color: white; font-family: Poppins; font-weight: 800; font-size: 160%; margin-left: 3%;">Alert!</span>   
                    </v-toolbar>
                </v-card-title>
                <v-divider></v-divider>
                <v-card-text style="font-family: Poppins; font-size: 100%; color: #001D38; margin-top: 5%"> Are you sure want to delete this data? </v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="#EEEEEE" large style="font-family: Poppins; font-size: 20px; text-transform: capitalize; font-weight: 900; color:#001D38;" @click="dialogConfirm = false;"> Cancel </v-btn>
                    <v-btn color="#EF5350" large style="font-family: Poppins; font-size: 20px; text-transform: capitalize; font-weight: 700; color:#EEEEEE;" @click="deleteData">Yes</v-btn> 
                </v-card-actions>
                </v-card>
        </v-dialog>
        <V-snackbar v-model="snackbar"  style="font-family: Poppins;" :color="color" timeout="2000" top>{{ error_message }}</v-snackbar> 
    </v-main>
</template>

<style lang="css">
    @import url('https://fonts.googleapis.com/css2?family=Urbanist:wght@800&display=swap');
    @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400&display=swap');

    thead tr th span {
        font-size: 160% !important;
        font-family: Poppins !important;
        letter-spacing: 2%;
        font-weight: 600;
        color: black !important;
    }

    tbody tr td{
        font-size: 100% !important;
        font-family: Poppins !important;
        letter-spacing: 2%;
        font-weight: 100;
        color: black !important;
    }

    .textfield{
        font-family: Poppins !important;
        color: black;
    }

    .v-list-item__title{
        font-family: Poppins !important;
        color: black;
    }

</style>

<script>
//tambahkan code untuk import database reference dan fungsi bawaan dari firebase database

export default {
    name: "MerchandisePage",
    data () {
        return {
            load: false,
            search: '',
            filter: '',
            snackbar: false,
            error_message: '',
            color: '',
            headers: [
                {text: "Merchandies Name", value: "merchandise"},
                {text: "Artist Name", value: "artist"},
                {text: "Price", value: "price"},
                {text: "Stock", value: "stock"},
                {text: "Packaging", value: "package"},
                {text: "", value: "actions"},
            ],
            type:[
                'Hot',
                'Cool',
                'Cold',
            ],
            inputRules: [
                (v) => !!v || 'Must be filled!',
            ],
            merchandise : new FormData,
            merchandises: [],
            dialog: false,
            dialogConfirm: false,
            form: {
                merchandise: '',
                artist: '',
                price: '',
                stock: '',
                package: ''
            },
            formType: 0,
            selectedId: '',
        }
    },

    created() {
        // tambahkan fungsi untuk retrieve data

    },

    methods: {
        saveData() {
            //tambahkan fungsi untuk create dan update data
            
        },

        editData(item){
            this.dialog = true; 
            this.formType = -1; 
            this.form = Object.assign({}, item);
            this.selectedId = item.id;
        },

        deleteData() {
            //tambahkan fungsi untuk delete data
            
        },

        closeDialog() {
            this.dialog = false;
            this.formType = 0;
            this.$refs.form.reset();
        }
    },

    computed: {
        formTitle() {
            return this.formType === 0 ? "Create Merchandise" : "Update Merchandise";
        },
    },
}
</script>

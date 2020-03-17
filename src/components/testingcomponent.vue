<template>
    <v-card class="view-user px-4 py-2 mb-8">
        <v-form ref="form" v-model="valid" lazy-validation @submit.prevent="validate">
            <v-container>
                <v-row wrap mb-4 class="mb-6">
                    <!--Left Column-->
                    <v-flex xs12 md5>
                        <!--First name-->
                        <v-row no-gutters wrap xs12>
                            <v-flex md3 required>
                                <v-subheader>First name</v-subheader>
                            </v-flex>
                            <v-flex md9>
                                <v-text-field v-model="user.first_name" dense outlined :rules="[isRequired]" />
                            </v-flex>
                        </v-row>
                        <!--Surname-->
                        <v-row no-gutters wrap xs12>
                            <v-flex md3 required>
                                <v-subheader>Surname</v-subheader>
                            </v-flex>
                            <v-flex md9>
                                <v-text-field v-model="user.surname" dense outlined :rules="[isRequired]" />
                            </v-flex>
                        </v-row>
                        <!--Email-->
                        <v-row no-gutters wrap xs12>
                            <v-flex md3 required>
                                <v-subheader>Email</v-subheader>
                            </v-flex>
                            <v-flex md9>
                                <v-text-field v-model="user.email" dense outlined :rules="[isRequired, isEmail]" />
                            </v-flex>
                        </v-row>
                        <!--Username-->
                        <v-row no-gutters wrap xs12>
                            <v-flex md3 required>
                                <v-subheader>Username</v-subheader>
                            </v-flex>
                            <v-flex md9>
                                <v-text-field v-model="user.username" dense outlined :rules="[isRequired]" />
                            </v-flex>
                        </v-row>
                        <!--Password-->
                        <v-row no-gutters wrap xs12>
                            <v-flex md3 :required="isNew">
                                <v-subheader>Password</v-subheader>
                            </v-flex>
                            <v-flex md9>
                                <v-text-field v-model="user.password" type="password" dense outlined :rules="isNew ? [isRequired] : []" />
                            </v-flex>
                        </v-row>
                        <!--Confirm Password-->
                        <v-row no-gutters wrap xs12>
                            <v-flex md3 :required="needPasswordConfirmation">
                                <v-subheader>Confirm Password</v-subheader>
                            </v-flex>
                            <v-flex md9>
                                <v-text-field v-if="!needPasswordConfirmation" disabled dense outlined />
                                <v-text-field v-if="needPasswordConfirmation" v-model="user.password_confirmation" type="password" dense outlined :rules="[isMatchingPassword]" />
                            </v-flex>
                        </v-row>
                        <!--Job Role-->
                        <v-row no-gutters wrap xs12>
                            <v-flex md3>
                                <v-subheader>Job Role</v-subheader>
                            </v-flex>
                            <v-flex md9>
                                <v-select v-model="user.job_role" :items="job_roles" item-text="title" item-value="value" dense outlined />
                            </v-flex>
                        </v-row>
                        <!--Start Date-->
                        <v-row no-gutters wrap xs12 required>
                            <v-flex md3 required>
                                <v-subheader>Start Date</v-subheader>
                            </v-flex>
                            <v-flex md9>
                                <v-menu v-model="startDate_menu" :close-on-content-click="false" transition="scale-transition" offet-y>
                                    <template v-slot:activator="{ on }">
                                        <v-text-field append-icon="date_range" readonly dense outlined :value="getStartDate" v-on="on" :rules="[isRequired]" />
                                    </template>

                                    <v-date-picker v-model="user.start_date" no-title @input="startDate_menu = false" />
                                </v-menu>
                            </v-flex>
                        </v-row>
                        <!--End Date-->
                        <v-row no-gutters wrap xs12>
                            <v-flex md3>
                                <v-subheader>End Date</v-subheader>
                            </v-flex>
                            <v-flex md9>
                                <v-menu v-model="endDate_menu" :close-on-content-click="false" transition="scale-transition" offet-y>
                                    <template v-slot:activator="{ on }">
                                        <v-text-field append-icon="date_range" readonly dense outlined :value="getEndDate" v-on="on" :rules="[isAfterStartDate]" />
                                    </template>

                                    <v-date-picker v-model="user.end_date" no-title @input="endDate_menu = false" :min="user.start_date" />
                                </v-menu>
                            </v-flex>
                        </v-row>
                        <!--Phone numbers-->
                        <v-row no-gutters wrap xs12>
                            <v-flex md3 required>
                                <v-subheader>Office No.</v-subheader>
                            </v-flex>
                            <v-flex md9>
                                <v-text-field v-model="user.landline_number" dense outlined :rules="[isRequired, isTelephone]" />
                            </v-flex>
                        </v-row>
                        <v-row no-gutters wrap xs12>
                            <v-flex md3>
                                <v-subheader>Mobile No.</v-subheader>
                            </v-flex>
                            <v-flex md9>
                                <v-text-field v-model="user.mobile_number" dense outlined :rules="[isTelephone]" />
                            </v-flex>
                        </v-row>
                    </v-flex>

                    <v-spacer/>

                    <!--Right Column-->
                    <v-flex xs12 md5>
                        <!--PO Management Limits-->
                        <h2 class="subtitle-1 text-uppercase">PO Management Limits</h2>
                        <!--Agency Limit-->
                        <v-row no-gutters wrap xs12>
                            <v-flex md3>
                                <v-subheader>Agency</v-subheader>
                            </v-flex>
                            <v-flex md9>
                                <v-text-field v-model="user.agency_limit" type="number" prefix="£" dense outlined :rules="[isCurrency]" />
                            </v-flex>
                        </v-row>
                        <!--Sub-Contractor Limit-->
                        <v-row no-gutters wrap xs12>
                            <v-flex md3>
                                <v-subheader>Sub-Contractor</v-subheader>
                            </v-flex>
                            <v-flex md9>
                                <v-text-field v-model="user.subcontractor_limit" type="number" prefix="£" dense outlined :rules="[isCurrency]" />
                            </v-flex>
                        </v-row>
                        <!--Supplier Limit-->
                        <v-row no-gutters wrap xs12>
                            <v-flex md3>
                                <v-subheader>Supplier</v-subheader>
                            </v-flex>
                            <v-flex md9>
                                <v-text-field v-model="user.supplier_limit" type="number" prefix="£" dense outlined :rules="[isCurrency]" />
                            </v-flex>
                        </v-row>
                        <!--PO Accounts Approval-->
                        <h2 class="subtitle-1 text-uppercase">PO Accounts Approval</h2>
                        <!--Authorised By-->
                        <v-row no-gutters wrap xs12>
                            <v-flex md3>
                                <v-subheader>Authorised by:</v-subheader>
                            </v-flex>
                            <v-flex md9>
                                <v-select v-model="user.authorised_by" :items="users" item-value="id" dense outlined>
                                    <!--Show custom item text for selected-->
                                    <template slot="selection" slot-scope="data">{{ data.item.first_name}} {{ data.item.surname }}</template>
                                    <!--Show custom item text for options-->
                                    <template slot="item" slot-scope="data">{{ data.item.first_name}} {{ data.item.surname }}</template>
                                </v-select>
                            </v-flex>
                        </v-row>
                        <!--Buttons!-->
                        <v-row wrap justify="end">
                            <!--Pushes buttons to the right-->
                            <v-spacer />
                            <!--Clear form-->
                            <v-btn color="primary" class="mr-4" outlined @click="reset">Clear</v-btn>
                            <!--Submit and validate-->
                            <v-btn :disabled="!valid" type="submit" color="primary" class="mr-4">Submit</v-btn>
                        </v-row>

                        <!-- TODO: extract into common component -->
                        <div v-if="!isNew">
                            <!--Comments Section-->
                            <v-divider />
                            <h2 class="subtitle-1 text-uppercase">Comments</h2>
                            <!--Add new comment-->
                            <v-row no-gutters wrap xs12>
                                <v-textarea v-model="add_comment" placeholder="Add comment" append-icon="add_circle" :rows="1" dense outlined @click:append="saveComment"></v-textarea>
                            </v-row>
                            <!--View comments-->

                            <v-row v-for="comment in comments" :key="comment.id" no-gutters wrap xs12>
                                <!--Loop through all the comments, and only enable editing (NOT readonly) when the index matches up-->
                                <v-textarea  v-model="comment.message" prepend-inner-icon="comment" type="text" dense outlined :readonly="comment.id !== enableEdit">
                                    <!--Header stuff-->
                                    <template v-slot:label>
                                        <div>
                                            Posted by User {{ comment.user_id }} on
                                            {{ comment.created_at }}
                                        </div>
                                    </template>
                                    <!--Actions-->
                                    <!--Show the edit and delete actions by default-->
                                    <template v-if="enableEdit !== comment.id" slot="append">
                                        <v-icon @click="editComment(comment)">edit</v-icon>
                                        <v-icon @click="deleteComment(comment.id)">delete</v-icon>
                                    </template>
                                    <!--Show the save/done and cancel/clear actions when editing-->
                                    <template v-if="enableEdit === comment.id" slot="append">
                                        <v-icon @click="updateComment(comment)">done</v-icon>
                                        <v-icon @click="revertComment(comment)">clear</v-icon>
                                    </template>
                                </v-textarea>
                            </v-row>
                            <v-btn v-if="showMoreComments" v-on:click="getComments()" class="ma-2" tile outlined color="primary">
                                <v-icon left>mdi-pencil</v-icon> Load more Comments
                            </v-btn>
                        </div>
                    </v-flex>
                </v-row>

                <v-divider v-if="!isNew" />

                <!--Buttons!-->
                <v-row v-if="!isNew" wrap justify="end" class="mt-6">
                    <!--Pushes buttons to the right-->
                    <v-spacer />

                    <v-flex v-if="user.status">
                        User Status: Active

                        <v-btn color="primary" class="ml-4" outlined @click="deactivate">Deactivate User</v-btn>
                    </v-flex>
                    <v-flex v-if="!user.status">
                        User Status: Inactive

                        <v-btn color="primary" class="ml-4" outlined @click="activate">Activate User</v-btn>
                    </v-flex>
                </v-row>
            </v-container>
        </v-form>
        <v-dialog v-model="dialog">
            {{ dialogText }}

            <v-btn color="primary" class="mr-4" outlined @click="dialog = false">No</v-btn>
            <v-btn color="primary" class="mr-4" @click="confirmDialog">Yes</v-btn>
        </v-dialog>
    </v-card>
</template>

<script>
import { callApi, getUser } from '../../plugins/api'

export default {
    name: "ViewUser",
    data() {
        return {
            commentPage: 0,
            dialog: false,
            dialogFunction: null,
            dialogText: '',
            //  This is pulled from the (api) data and used within the form
            user: {
                first_name: null,
                surname: null,
                email: null,
                username: null,
                password: null,
                job_role: null,
                start_date: null,
                end_date: null,
                mobile_number: null,
                landline_number: null,
                supplier_limit: null,
                agency_limit: null,
                subcontractor_limit: null,
                authorised_by: null
            },
            //  The form is valid by default
            valid: true,
            //  Hide the Start/End date menus
            startDate_menu: false,
            endDate_menu: false,
            job_roles: [
                { title: '', value: '0' },
                { title: 'Project Manager', value: '1' },
                { title: 'Site Manager', value: '2' },
                { title: 'Chairman', value: '3' },
                { title: 'Projects Coordinator', value: '4' },
                { title: 'Design Technician', value: '5' },
            ],
            //  A list of users
            users: [],
            //  A list of comments
            comments: [],
            //  Add a new comment
            add_comment: null,
            // Original comment when editing
            old_comment: null,
            // Show "more comments" button
            showMoreComments: false,
            //  Set the index for the comment to be edited
            enableEdit: null,
        }
    },
    computed: {
        isNew() {
            return !this.userId
        },
        userId() {
            return this.$route.params.id
        },
        needPasswordConfirmation() {
            return this.isNew || !!this.user.password
        },
        getStartDate() {
            return this.user.start_date
        },
        getEndDate() {
            return this.user.end_date
        }
        
    },
    methods: {
        //  Get the user data from the API
        getUser() {
            if (this.isNew) return

            //  Fetch the user's data, passing in the ID
            callApi('GET', 'users/' + this.userId, data => {
                this.user = data
            })
        },
        //  Saves the data to the API
        saveUser() {
            if (this.isNew) {
                return callApi('POST', 'users', this.user, data => {
                    alert('User added')
                    this.$router.push('/users/' + data.id)
                })
            }

            //  Update the data, passing in the ID
            callApi('PATCH', 'users/' + this.userId, this.user, data => {
                this.user = data
                //  TODO: remove this for the overall success
                alert('User updated')
            })
        },
        //  Get a list of users to use
        getUsers() {
            //  Get the Users list
            callApi('GET', 'users', data => {
                //  Filter out the current user from the list
                this.users = data.filter(u => u.id != this.userId)
            })
        },
        //  Get a list of comments for current user
        getComments() {
            if (this.isNew) return

            //Get the current comment page
            const page = this.commentPage + 1

            //  Get the Comments list
            callApi('GET', `comments/User/${this.userId}?page=${page}`, pagination => {
                this.comments = this.comments.concat(pagination.data)
                this.showMoreComments = pagination.last_page > pagination.current_page
                this.commentPage = pagination.current_page
            })
        },
        //  Saves the comment to the API
        saveComment() {
            if(this.add_comment){
                const body = new FormData();

                //  Get the submitted fields
                body.append('user_id', getUser().id);
                body.append('type', '0');
                body.append('commentable_id', this.userId);
                body.append('commentable_type', '0');
                body.append('message', this.add_comment);

                //  Update the data, passing in the ID
                callApi('POST', 'comments', body, data => {
                    console.log(data)
                    //  Has the API submitted the data?
                    if (data.status) {
                        this.getComments()
                        this.add_comment = null
                    } else {
                        //  New popup?
                        alert('Comment failed to add.')
                    }
                })
            }
        },
        //  Edits the comment in the API
        editComment(c) {
            //  Set the old comment, as a backup
            this.old_comment = c.message
            // This sets which comment loop row to edit
            this.enableEdit = c.id
        },
        //  Saves the update (PATCH) to the API
        updateComment(c) {
            //  Update the data, passing in the ID
            callApi('PATCH', 'comments/' + c.id, c, data => {
                //  BIG TODO: sort out the status catch error checks
                //  MOAR TODO: get the snackbar sorted
                //  Has the API submitted the data?
                if (data.status) {
                    this.getComments()
                } else {
                    alert('Failed to update')
                }
            })

            //  Turn off the edits
            this.enableEdit = null
        },
        //  Undos the comments made - probably just a reload of this comment
        revertComment(c) {
            //  Restore the message from backup
            c.message = this.old_comment
            //  Disable editing
            this.enableEdit = null
            //  Clear the backup
            this.old_comment = null
        },
        //  Deletes the comment from the API
        deleteComment(c) {
            callApi('DELETE', 'comments/' + c, data => {
                //  Has the API submitted the data?
                if (data.status) {
                    this.getComments()
                } else {
                    //  TODO: Snackbar?
                    alert('Failed to delete')
                }
            })
        },
        showDialog(fn, text) {
            this.dialog = true
            this.dialogFunction = fn
            this.dialogText = text
        },
        confirmDialog() {
            this.dialog = false
            this.dialogFunction()
        },
        activate() {
            this.showDialog(this.confirmActivate, 'Are you sure you want to activate this user?')
        },
        confirmActivate() {
            callApi('POST', `users/${this.userId}/activate`, data => {
                Object.assign(this.user, data)
                alert('User activated')
            })
        },
        deactivate() {
            this.showDialog(this.confirmDeactivate, 'Are you sure you want to deactivate this user?')
        },
        confirmDeactivate() {
            callApi('POST', `users/${this.userId}/deactivate`, data => {
                Object.assign(this.user, data)
                alert('User deactivated')
            })
        },
        //  Validation rules (TODO: extract to mixin)
        isRequired: v => !!v || 'Required',
        isEmail: v => (!v || /.+@.+\..+/.test(v)) || 'Invalid email address',
        isCurrency: v => (!v || /^\d*(\.\d{1,2})?$/.test(v)) || 'Up to 2 decimal places',
        isTelephone: v => (!v || /^[0]\d{9,10}$/.test(v)) || '10/11 digits only',
        isMatchingPassword(v) {
            return v == this.user.password || 'Does not match'
        },
        isAfterStartDate(v) {
            if (!v) return true

            const start = new Date(this.user.start_date)
            const end = new Date(this.user.end_date)
            return end > start || 'Must be after start date'
        },
        //  Validate the form
        validate() {
            if(this.$refs.form.validate()) {
                 if(new Date(this.user.end_date) >= new Date(this.user.start_date)){
                     this.saveUser()
              }else{
                 alert("Endate must me ahead or equal to startdate"); 
              }
               
            } else {
                //  TODO: change this for snackbar
                alert('Submit Failed')
            }
        },
        //  Reset the form
        reset() {
            //  Reset the form
            this.$refs.form.reset()
            //  Reset the start and end dates
            this.user.start_date = null
            this.user.end_date = null
        },
         
    },
    created() {
        //  Get the users as soon as possible
        this.getUser();
        this.getUsers();
        this.getComments();
    },
   
}
</script>
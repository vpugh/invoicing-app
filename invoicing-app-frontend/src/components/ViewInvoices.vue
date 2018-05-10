<template>
    <div>
        <div class="container">
            <div class="tab-pane fade show active">
                <div class="row">
                    <div class="col-md-12">
                        <h3 class="table-header">Here is a list of your invoices</h3>
                        <table class="table">
                            <thead style="margin-bottom: 20px;">
                                <tr>
                                    <th scope="col">Invoice #</th>
                                    <th scope="col">Invoice Item</th>
                                    <th scope="col">Status</th>
                                    <th scope="col"></th>
                                </tr>
                            </thead>
                            <tbody>
                                <template v-for="(invoice, index) in invoices">
                                    <tr :key="invoice.id">
                                        <th scope="row">{{ invoice.id }}</th>
                                        <td>{{invoice.name}}</td>
                                        <td v-if="invoice.paid == 0">Unpaid</td>
                                        <td v-else>Paid</td>
                                        <td><button type="button" class="btn btn-success" data-toggle="modal" data-target="#invoiceModal" @click="singleInvoice(invoice.id)">View Invoice</button></td>
                                    </tr>
                                </template>
                                <form @submit.prevent="onSubmit" v-for="transaction in transactions">
                                    <!-- Modal -->
                                <div class="modal fade" id="invoiceModal" tabindex="-1" role="dialog" aria-labelledby="invoiceModalLabel" aria-hidden="true">
                                    <div class="modal-dialog" role="document" >
                                        <div class="modal-content">
                                            <div class="modal-header">
                                                <input type="text" :value="transaction.name" style="border:none;font-weight:bold; font-size:1.4em;">
                                                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                                                    <span aria-hidden="true">&times;</span>
                                                </button>
                                            </div>
                                            <div class="modal-body">
                                                <div class="form-group">
                                                    <label for="">Item name</label>
                                                    <input type="text" id="transaction_name_modal" class="form-control" :value="transaction.name">
                                                </div>
                                                <div class="form-group">
                                                    <label for="">Price ($)</label>
                                                    <input type="numeric" id="transaction_price_modal" class="form-control" :value="transaction.price">
                                                </div>
                                                <div class="form-group">
                                                    <div :class="{'paid': transaction.paid != 0, 'unpaid': transaction.paid == 0}">
                                                    <p v-if="transaction.paid == 0">Unpaid</p>
                                                    <p v-else>Paid</p>
                                                    </div>
                                                </div>
                                            </div>
                                            <div class="modal-footer">
                                                <button type="button" class="btn btn-secondary" data-dismiss="modal" @click="changePaid(transaction.paid)">Mark as Paid</button>
                                                <button type="button" class="btn btn-primary" data-dismiss="modal" v-on:click="saveTransaction()">Update Information</button>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                </form>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";

export default {
    name: "ViewInvoices",
    components: {},
    data() {
        return {
            invoices: [],
            transactions: [{"name": "", "id": "", "price": "", "paid": ""}
            ],
            user: this.$route.params.user,
            invoice: this.$route.params.invoices,
            selectedInvoice: ''
        };
    },
    methods: {
        paidValidate() {
        // checks all the form params are set and the passwords match
        if (this.model.password != this.model.c_password){
            return false;
        }

        return true;
        },
        singleInvoice(invoice_id) {
            this.selectedInvoice = invoice_id;
            console.log(this.selectedInvoice);

            var self = this;
            console.log(self);

            axios.get(`http://localhost:3128/invoice/user/${this.user.id}/${this.selectedInvoice}`).then(res => {
                this.transactions = res.data.transactions;
                self.transactions.fillItem = this.transactions;
            })

            let name = document.getElementById("transaction_name_modal").value;
            let price = document.getElementById("transaction_price_modal").value;
            let paid = document.getElementById("transaction_price_modal").value;
        },
        changePaid(paid) {
            if (currentPaid != 0 ) {
                this.transactions.push({
                    id: this.nextTxnId,
                    name: name,
                    price: price
                });
                this.nextTxnId++;
                this.calcTotal();

                // Clear their values
                document.getElementById("txn_name_modal").value = "";
                document.getElementById("txn_price_modal").value = "";
            }
        },
        updateTransaction() {

        }
    },
    mounted() {
        axios.get(`http://localhost:3128/invoices/${this.user.id}`).then(res => {
            if (res.data.status == true) {
                // console.log(res.data.invoices);
                this.invoices = res.data.invoices;
                
            }
        });
    }
        
};
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #ffffff;
}
.tab-pane {
  margin-top: 20px;
}

/* Table Styles */

.table-header {
    margin-bottom: 30px;
}
.table td,
.table th {
    vertical-align: middle;
    padding: 1em;
}

/* Modal Styles  */
.modal-body input {
    border:none;
    border-bottom: 1px solid;
    border-radius: 0;
    margin-bottom: 30px;
    margin-left: 0;
    padding-left: 0;
    padding-top: 0;
    color: #222;
}
.modal label {
    text-align: left;
    width: 100%;
    font-size: .9em;
    color: #828282;
}
.modal-body label {
    margin-bottom: 0;
}
.unpaid {
    background: red;
    color: white;
    padding: 16px 0 1px 0;
    font-weight: bold;
    font-size: 1.2em;
    text-transform: uppercase;
}
.paid {
    background: green;
    color: white;
    padding: 16px 0 1px 0;
    font-weight: bold;
    font-size: 1.2em;
    text-transform: uppercase;
}
</style>
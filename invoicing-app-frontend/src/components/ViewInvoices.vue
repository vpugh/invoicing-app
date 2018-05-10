<template>
  <div>

    <div class="container">

      <div class="tab-pane fade show active">
          <div class="row">
              <div class="col-md-12">
                  <h3 class="mb-4">Here's a list of your Invoices</h3>
                  <table class="table">
                    <thead>
                      <tr>
                        <th scope="col">Invoice #</th>
                        <th scope="col">Invoice Name</th>
                        <th scope="col">Status</th>
                        <th scope="col"></th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="invoice in invoices" :key="invoice.id">
                          <th scope="row">{{ invoice.id }}</th>
                          <td>{{ invoice.name }}</td>
                          <td v-if="invoice.paid == 0 "> Unpaid </td>
                          <td v-else> Paid </td>
                          <td ><a class="btn btn-success" @click="viewInvoice(invoice.id)" data-toggle="modal" data-target="#transactionModal">VIEW INVOICE</a>
                          </td>
                      </tr>
                    </tbody>
                    <!-- Modal -->
                        <div class="modal fade" id="transactionModal" tabindex="-1" role="dialog" aria-labelledby="transactionModalLabel" aria-hidden="true" v-for="transaction in transactions" :key="transaction.id">
                          <div class="modal-dialog" role="document">
                            <div class="modal-content">
                              <div class="modal-header">
                                <h5 class="modal-title" id="exampleModalLabel">{{transaction.name}}</h5>
                                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                                  <span aria-hidden="true">&times;</span>
                                </button>
                              </div>
                              <div class="modal-body">
                                <div class="form-group">
                                  <label for="">Item name</label>
                                  <input type="text" id="txn_name_modal" class="form-control" :value="transaction.name">
                                </div>

                                <div class="form-group">
                                  <label for="">Price ($)</label>
                                  <input type="numeric" id="txn_price_modal" class="form-control" :value="transaction.price">
                                </div>
                              </div>
                              <div class="modal-footer">
                                <button type="button" class="btn btn-secondary" data-dismiss="modal">Discard Transaction</button>
                                <button type="button" class="btn btn-primary" data-dismiss="modal" v-on:click="saveTransaction()">Save transaction</button>
                              </div>
                            </div>
                          </div>
                        </div>
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
  data() {
    return {
      invoices: [],
      transactions: [
        {
          "id": '',
          "invoice_id": '',
          "name": '',
          "paid": '',
          "price": ''
        }
      ],
      user: this.$route.params.user,
      currentLink: ''
    };
  },
  methods: {
    singleInvoice() {
      if (res.data.status == true) {
          // this.registerStatus = res.data.message;
          console.log(res.data.user);
          // now send the user to the next route
          this.$router.push({
            name: "SingleInvoice",
            params: { user: res.data.user, invoiceLocation: this.currentLink }
          });
        } else {
          this.status = res.data.message;
        }
    },
    viewInvoice(id) {
      console.log(id);
      this.currentId = id;
      console.log(this.currentId);

      axios
      .get(`http://localhost:3128/invoice/user/${this.user.id}/${this.currentId}`)
      .then(res => {
        console.log(res.data.transactions);
        this.transactions = res.data.transactions;
      });
    }
  },
  mounted() {
    axios
      .get(`http://localhost:3128/invoice/user/${this.user.id}`)
      .then(res => {
        if (res.data.status == true) {
          console.log(res.data.invoices);
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
</style>
<template>
    <div class="home-container">
        <div class="page-header" v-if="currentView === 'accounts'">
            <h3 class="dt-inline-block">Accounts Overview</h3>
            <small>As of {{new Date().toLocaleDateString('default', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' })}}</small>
        </div>
       <div class="accounts-section" v-if="currentView === 'accounts'">
           <div class="container-fluid">
                <div class="row">
                    <div class="col-md-8">
                        <div class="accounts-info">
                            <div class="accounts-list">
                                <div class="account-record" v-on:click="openTransactions(account.id)" v-bind:key="account.id" v-for="account in accounts">
                                    <div class="row align-items-center">
                                        <div class="col-md-1">
                                            <img v-show="account.type === 'Personal Account'" width="40px" src="../../../content/images/credit-card.png" />
                                            <img v-show="account.type === 'Direct Quarterly Savings'" width="40px" src="../../../content/images/piggy-bank_grenn.png" />
                                        </div>
                                        <div class="col-md-4">
                                            <div>{{account.name}}</div>
                                            <small>{{account.type}}</small>
                                        </div>
                                        <div class="col-md-4">
                                            <span>{{account.number}}</span>
                                        </div>
                                        <div class="col-md-2">
                                            <span> {{account.balance | toCurrency}}</span>
                                        </div>
                                        <div class="col-md-1">
                                            <font-awesome-icon icon="chevron-right"></font-awesome-icon>
                                        </div>
                                    </div>                                    
                                </div>
                                <div class="ad-banner">
                                    <div class="row align-items-center">
                                        <div class="col-md-2">
                                            <img width="100px" src="../../../content/images/link-card.png" />
                                        </div>
                                        <div class="col-md-10">
                                            <h4>Add accounts with other banks to your account overview</h4>
                                            <p>Also have accounts with another bank ? You can give ING, Rabobank, ASN, SNS or RegioBank
                                                 permission to add these accounts to your ABN AMRO account overview, so you can keep track
                                                  of all your acounts at all times. </p>
                                            <div class="float-right">
                                                <button class="btn btn-light">Hide</button>  
                                                <button class="btn btn-ib">Add other bank</button>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <p>Questions or in need of help, You can reach us 24/7.</p>
                        <a href>Contact</a>
                    </div>                    
                </div>
            </div>
       </div>
       <div class="transactions-container" v-if="currentView === 'transactions'">
           <div class="container-fluid">
               <div class="row">
                   <div class="col-md-12" style="margin-bottom: 15px">
                       <a class="bread-crumbs" href v-on:click.prevent="openAccountsList()">Accounts</a> > <a class="bread-crumbs" href>Summary</a>
                   </div>
               </div>
               <div class="row">
                   <div class="col-md-3 vr">
                       <div class="accounts-detail-column">
                           <div class="accounts-list-section">
                               <h4>Account details for</h4>
                               <select class="d-xs-block d-sm-none form-control" name="account" v-on:change="showTransactions($event.target.value)">
                                   <option v-bind:key="account.id" v-for="account in accounts" v-bind:value="account.id"> {{account.name}} </option>
                               </select>
                               <div class="account-card d-none d-sm-block" v-bind:class="{'active': account.id === currentSelectedAccount}" v-on:click="showTransactions(account.id)" v-bind:key="account.id" v-for="account in accounts">
                                    <div class="row align-items-center">
                                        <div class="col-md-4">
                                            <h4>{{account.name}}</h4>
                                            <small>{{account.number}}</small>
                                        </div>
                                        <div class="col-md-6 text-right">
                                            <h4>{{account.balance | toCurrency}}</h4>
                                            <small>{{account.type}}</small>
                                        </div>
                                        <div class="col-md-2">
                                            <font-awesome-icon icon="chevron-right"></font-awesome-icon>
                                        </div>
                                    </div>
                               </div>
                           </div>
                           <div class="transactions-filters-section">
                                <h4>Transactions Filter</h4>
                                <select class="d-xs-block d-sm-none form-control" name="filter">
                                   <option value="all"> All </option>
                                   <option value="all"> Pending </option>
                                   <option value="all"> Rejected </option>
                                   <option value="all"> Scheduled and standing </option>
                                   <option value="all"> Direct debits </option>
                               </select>
                                <div class="form-group d-none d-sm-block">
                                    <input type="checkbox" id="txfilter1" name="txfilter" />
                                    <label for="txfilter1">Pending</label>
                                </div>
                                <div class="form-group d-none d-sm-block">
                                    <input type="checkbox" id="txfilter2" name="txfilter" />
                                    <label for="txfilter2">Rejected</label>
                                </div>
                                <div class="form-group d-none d-sm-block">
                                    <input type="checkbox" id="txfilter3" name="txfilter" />
                                    <label for="txfilter3">Scheduled and standing</label>
                                </div>
                                <div class="form-group d-none d-sm-block">
                                    <input type="checkbox" id="txfilter4" name="txfilter" />
                                    <label for="txfilter4">Direct debits</label>
                                </div>
                           </div>
                           <button class="btn btn-ib col-md-12 d-none d-sm-block">New Transfer</button>
                       </div>            
                   </div>
                   <div class="col-md-6 vr">
                       <div class="d-xs-block d-sm-none mt-20"></div>
                       <div class="transactions-detail-column">
                           <div class="row">
                               <div class="col-md-6">
                                   <h4 class="header">Transactions</h4>
                                </div>
                                <div class="col-md-6 text-right">
                                    <h4 class="header">
                                        Account settings 
                                        <span><font-awesome-icon icon="cog"></font-awesome-icon></span>
                                    </h4>                                    
                                </div>
                            </div>
                            <div class="row">
                                <div class="col-md-12">
                                    <div class="transactions-list">
                                        <div v-if="!transactionsLoaded" class="text-center"><h4>Loading transactions ...</h4></div>
                                        <div class="transaction-group" v-bind:key="key" v-for="(trnxs, key) in transactions">                                                
                                            <span class="date">{{key}}</span>
                                            <div class="card">
                                                <table>
                                                    <tr class="transaction" v-bind:key="trnx.id" v-for="trnx in trnxs">
                                                        <td>&euro;</td> 
                                                        <td>{{trnx.description}}</td> 
                                                        <td style="text-align: right">
                                                            <b>{{ getAmount(trnx)  | toCurrency }}</b>
                                                        </td> 
                                                    </tr>
                                                </table>                                                    
                                            </div>                                                
                                        </div>
                                    </div>
                                </div>
                            </div>
                       </div>
                   </div>
                   <div class="col-md-12 d-xs-block d-sm-none text-center">
                       <button class="btn btn-ib">New Transfer</button>
                    </div>            
                   <div class="col-md-3">
                       <div class="ads-column">
                           <h4 class="header">For Daily Banking Matters</h4>
                           <div class="block one"> 
                               <img width="40px" src="../../../content/images/u141.svg" />
                               <h4>Call Now 0900 - 00 24</h4>
                            </div>
                            <div class="block two">
                                <h4> Preffered Banking</h4>
                                <button class="btn btn-ib col-md-12">Make an appointment (in Dutch)</button>
                            </div>
                            <div class="block">
                                <ul class="ad-menu">
                                    <li>your personal adviser</li>
                                    <li>interactive clinics</li>
                                    <li>access to Aspire Lounges at Schiphol Airport</li>
                                    <li>tips and information on investments and pensions</li>
                                </ul>
                            </div>
                       </div>
                   </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="ts" src="./home.component.ts">
</script>

<style scoped>
.page-header {
  height: 150px;
  width: 100%;
  background-image: linear-gradient(-296deg, #005e5d, #00857a);
  padding-top: 35px;
  padding-left: 100px;
  color: #fff;
  font-weight: bold;
}
.accounts-info {
  margin-left: 65px;
  position: relative;
  top: -50px;
  padding: 0 20px;
}
.accounts-list {
  border-radius: 3px;
}
.account-record {
  background-color: #fff;
  padding: 10px 20px;
  border-bottom: 1px solid #efefef;
  cursor: pointer;
}
.account-record:first-child {
  border-top: 1px solid #efefef;
}

.account-record:last-child {
  border-bottom: 1px solid #efefef;
}

.ad-banner {
  margin-top: 20px;
  border: 1px dashed #efefef;
  padding: 20px;
  background: #F9F9F9;
}
.ad-banner h4 {
  font-size: 20px;
  font-weight: 600;
}

.transactions-container {
  margin-top: 10px;
}
.transactions-container .col-md-3.vr,
.transactions-container .col-md-6.vr {
  border-right: 1px solid #efefef;
}
.transactions-section {
  background-color: #f6f4f7;
}
.transactions-section .block {
  background-color: #fff;
  padding: 15px;
  margin-bottom: 15px;
}
.transactions-section .header {
  text-align: center;
  border: 1px solid #efefef;
  font-weight: bold;
  padding: 5px 10px;
}
.transactions-section .header h4 {
  color: #00857a;
  font-weight: bold;
}
.transactions-section .body {
  padding: 20px;
  border: 1px solid #efefef;
  border-top: none;
}
.transactions-section .body a {
  color: #00857a;
  font-size: 14px;
  display: block;
}
.transactions-section .body .heading {
  font-size: 16px;
}
.transaction-group {
  margin-bottom: 20px;
}
.transaction-group .date {
  color: #00857a;
  font-weight: 600;
}
.transaction-group .card {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  transition: 0.3s;
}
.transaction-group .card .transaction {
  border-bottom: 1px solid #efefef;
}
.transaction-group .card .transaction:last-child {
  border-bottom: none;
}
.transaction-group .card .transaction > td {
  padding: 5px;
}

.accounts-detail-column > .accounts-list-section {
  margin-bottom: 20px;
}

.accounts-detail-column > .accounts-list-section > h4 {
  font-weight: 600;
}

.accounts-detail-column > .accounts-list-section > .account-card {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  padding: 5px;
  border-left: 5px solid #fff;
  margin-bottom: 10px;
  cursor: pointer;
}

.accounts-detail-column > .accounts-list-section > .account-card.active {
  border-left: 5px solid #00857a;
}

.accounts-detail-column > .accounts-list-section > .account-card h4 {
  font-size: 14px;
  font-weight: bold;
  margin-bottom: 0px;
}

.accounts-detail-column > .accounts-list-section > .account-card small {
  font-size: 10px;
}

.transactions-filters-section .form-group {
  margin-bottom: 0px;
}

.transactions-filters-section > h4 {
  font-weight: 600;
}

.transactions-filters-section label {
  color: #00857a;
}

.transactions-detail-column .header {
  font-weight: 600;
}

.ads-column .header {
  font-weight: 600;
}

.ads-column .block {
  padding: 20px;
  margin-bottom: 20px;
}

.ads-column .block.one {
  text-align: center;
  background: #ffd200;
}

.ads-column .block.one h4 {
  display: inline;
  font-size: 16px;
  font-weight: 600;
}

.ads-column .block.two {
  color: #fff;
  text-align: center;
  background: #00605e;
}

.ads-column .block.two h4 {
  font-weight: 600;
  font-size: 24px;
}

.ads-column ul.ad-menu {
  list-style: none;
  padding: 0px;
}

.ads-column ul.ad-menu li:before {
  content: '✓';
  color: #00857a;
  padding-right: 10px;
}

.bread-crumbs {
  color: #00605e;
}

.mt-20 {
    margin-top: 20px;
}
</style>

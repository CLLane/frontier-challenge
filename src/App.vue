<template>
<div id="app">
  <html>
    <head>
      <title>Accounts Coding Test</title>
    </head>
    <body>
      <main>
        <header>
          <h2>Accounts</h2>
        </header>
        <section class="content">
          <article>
            <h3>Active</h3>
            <div class="active">
              <ul v-for="active in actives" :key="active">
                <li>
                  <label>Name:</label>
                  {{active.LastName}}, {{active.FirstName}}
                </li>
                <li>
                  <label>Email:</label>
                  {{active.Email}}
                </li>
                <li>
                  <label>Phone Number:</label>
                  {{formatPhoneNumber(active.PhoneNumber)}}
                </li>
                <li>
                  <label>Amount Due:</label>
                  {{formatAmountDue(active.AmountDue)}}
                </li>
                <li>
                  <label>Due Date:</label>
                  {{formatDate(active.PaymentDueDate)}}
                </li>
              </ul>
            </div>
          </article>
          <article>
            <h3>Overdue</h3>
            <div class="overdue">
              <ul v-for="overdue in overdues" :key="overdue">
                <li>
                  <label>Name:</label>
                  {{overdue.LastName}}, {{overdue.FirstName}}
                </li>
                <li>
                  <label>Email:</label>
                  {{overdue.Email}}
                </li>
                <li>
                  <label>Phone Number:</label>
                  {{formatPhoneNumber(overdue.PhoneNumber)}}
                </li>
                <li>
                  <label>Amount Due:</label>
                  {{formatAmountDue(overdue.AmountDue)}}
                </li>
                <li>
                  <label>Due Date:</label>
                  {{formatDate(overdue.PaymentDueDate)}}
                </li>
              </ul>
            </div>
          </article>
          <article>
            <h3>Inactive</h3>
            <div class="inactive">
              <ul v-for="inactive in inactives" :key="inactive">
                <li>
                  <label>Name:</label>
                  {{inactive.LastName}}, {{inactive.FirstName}}
                </li>
                <li>
                  <label>Email:</label>
                  {{inactive.Email}}
                </li>
                <li>
                  <label>Phone Number:</label>
                  {{formatPhoneNumber(inactive.PhoneNumber)}}
                </li>
                <li>
                  <label>Amount Due:</label>
                  {{formatAmountDue(inactive.AmountDue)}}
                </li>
                <li v-if="inactive.PaymentDueDate">
                  <label>Due Date:</label>
                  {{inactive.PaymentDueDate}}
                </li>
              </ul>
            </div>
          </article>
        </section>
      </main>
      <footer>
        <p>&copy;{{date}}</p>
      </footer>
    </body>
  </html>
</div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      actives: {},
      inactives: {},
      overdues: {},
      date: ""
    };
  },
  beforeMount() {
    this.getCustomers();
  },
  methods: {
    async getCustomers() {
      const res = await fetch(
        "https://frontiercodingtests.azurewebsites.net/api/accounts/getall"
      );
      const data = await res.json();
      this.actives = data.filter(el => el.AccountStatusId === 0);
      this.inactives = data.filter(el => el.AccountStatusId === 1);
      this.overdues = data.filter(el => el.AccountStatusId === 2);
      this.date = new Date().getFullYear();
    },
    formatPhoneNumber(phoneNumberString) {
      let cleaned = ("" + phoneNumberString).replace(/\D/g, "");
      let match = cleaned.match(/^(\d{3})(\d{3})(\d{4})$/);
      return "(" + match[1] + ") " + match[2] + "-" + match[3];
    },

    formatDate(dateString) {
      let sliceDate = dateString.slice(0, 10);
      let reversed = sliceDate.split("-").reverse();
      if (reversed[0].length < 2) {
        reversed[0] = "0" + reversed[0];
      }
      return reversed.join("/");
    },
    formatAmountDue(amountDueString) {
      let format = `$${amountDueString}`;
      return format;
    }
  }
};
</script>

<style>
main,
article,
section,
header,
footer {
  display: flex;
}
main,
article {
  flex-direction: column;
}
article {
  align-items: center;
}
section {
  justify-content: space-evenly;
}
header,
footer {
  justify-content: center;
}
ul {
  list-style-type: none;
  border: 3px solid whitesmoke;
  border-radius: 1em;
  padding: 0.75em;
}
li {
  padding: 2px;
}
.overdue,
.inactive,
.active {
  padding: 1em;
  border-radius: 1em;
}
.overdue {
  background-color: palevioletred;
}
.inactive {
  background-color: paleturquoise;
}
.active {
  background-color: palegreen;
}
@media screen and (max-width: 875px) {
  section {
    flex-direction: column;
  }
  article {
    width: 100%;
  }
}
</style>

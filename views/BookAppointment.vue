export default {
  name: "BookAppointment",
  data() {
    return {
      name: "",
      symptoms: "",
      selectedSlot: "",
      slots: []
    };
  },
  mounted() {
    fetch("https://yzmmifgs91.execute-api.us-east-1.amazonaws.com/dev/slots")
      .then(res => res.json())
      .then(data => {
        this.slots = data.filter(s => !s.isBooked).map(s => s.slot);
      })
      .catch(err => {
        console.error("Error fetching slots:", err);
      });
  },
  methods: {
    submitAppointment() {
      const payload = {
        patientName: this.name,
        symptoms: this.symptoms,
        slot: this.selectedSlot
      };

      fetch("https://yzmmifgs91.execute-api.us-east-1.amazonaws.com/dev/appointments", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ body: JSON.stringify(payload) })
      })
        .then(res => res.json())
        .then(() => {
          alert("Appointment booked!");
          this.name = "";
          this.symptoms = "";
          this.selectedSlot = "";
        });
    }
  }
};

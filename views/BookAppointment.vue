<script>
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
    fetch("https://nup89vg7a8.execute-api.us-east-1.amazonaws.com/test/slots")
      .then(res => res.json())
      .then(data => {
        // تعديل هنا: parse الـ body
        let slotsArr = data.body;
        if (typeof slotsArr === "string") {
          slotsArr = JSON.parse(slotsArr);
        }
        // خلي slots عبارة عن Array من الكائنات [{slot, isBooked}]
        this.slots = slotsArr.filter(s => !s.isBooked);
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
      fetch("https://nup89vg7a8.execute-api.us-east-1.amazonaws.com/test/appointments", {
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
        })
        .catch(err => {
          console.error("Error booking appointment:", err);
          alert("Failed to book appointment.");
        });
    }
  }
};
</script>


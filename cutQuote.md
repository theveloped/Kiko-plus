---
title: cutQuote
---

<script>
xhr = new XMLHttpRequest();
xhr.onreadystatechange = function() {
  console.log("[+] XHR received")
  console.log(xhr.readystate)
  console.log(xhr.status)
  if(xhr.readystate == 4 && xhr.status == 200) {
    document.getElementById('smartparts').innerHTML = xhr.responseText;
  }
};
xhr.open('GET', 'https://www.thevelop.io/app?apiKey=5738196700758016', true);
xhr.withCredentials = true;
xhr.send();
</script>

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Quisque pellentesque, leo a condimentum lacinia, metus leo suscipit lorem, in mattis dui lectus nec metus. Aliquam dictum quam a diam elementum lobortis. Mauris ac erat non tortor euismod iaculis nec id ligula. Vivamus imperdiet arcu sed molestie dictum. Aenean eu purus sit amet magna sodales venenatis. Ut vitae metus vestibulum, eleifend ligula ut, pulvinar ipsum. Integer sit amet commodo purus. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos.

<div id="smartparts" style="width: 400px; height: 400px;"></div>

Integer molestie purus vel cursus pharetra. Aliquam semper at lorem consectetur laoreet. Suspendisse pellentesque lacus non diam porttitor molestie. Donec urna sem, rutrum eget tortor congue, porttitor elementum lectus. Vestibulum arcu mauris, facilisis sed ex eu, accumsan fringilla ex. Integer congue nunc arcu, id volutpat lectus gravida eget. Fusce sed orci sed magna lobortis dapibus. Mauris porta, lectus sed molestie convallis, magna turpis pulvinar neque, a feugiat orci mauris tempor justo. Donec venenatis placerat odio, a hendrerit dolor imperdiet id. Phasellus nec pharetra eros. Phasellus rutrum, mi a rutrum tristique, tortor elit fermentum nunc, et tempus urna erat et nunc.

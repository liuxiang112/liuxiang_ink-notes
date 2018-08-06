#### cookie
````
const cookie = {
  setCookie (name, val, day) {
    let expireDate = new Date();
    expireDate.setDate(expireDate.getDate() + day);
    // let str = document.cookie;
    document.cookie += `${name}=${val};expires=${expireDate.toGMTString()}`;
  },
  getCookies () {
    let cookies = [];
    if(document.cookie) {
      let cookieArr = document.cookie.split(';');
      for (let i = 0;i<cookieArr.length;i++) {
        let keyArr = cookieArr[i].split("=");
        let name = keyArr[0];
        let value = keyArr[1];
        cookies.push({name,value});
      }
    } else {
      return false;
    }
    return cookies;
  },
  getCookie (name) {
    let cookies = this.getCookies();
    if (cookies) {
      for (let cookie of cookies) {
        if (cookie.name == name) {
          return cookie;
        }
      }
      return false;
    }
  },
  removeCookie (name) {
    let cookies = this.getCookies();
    if (cookies) {
      for(let cookie of cookies) {
      if(cookie.name === name) {
          setCookie(name, null, -99);
          break;
        }
      }
    } else {
      return false;
    }
  }
}
export default cookie;
````
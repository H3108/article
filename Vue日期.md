var now = new Date(); //当前日期
var nowDayOfWeek = now.getDay(); //今天本周的第几天
var nowDay = now.getDate(); //当前日
var nowMonth = now.getMonth(); //当前月
var nowYear = now.getFullYear(); //当前年

//获得本周的开端日期
function getWeekStartDate() {
  let weekStartDate = new Date(nowYear, nowMonth, nowDay - nowDayOfWeek);
  return formatDate(weekStartDate);
}
//获得本周的停止日期
function getWeekEndDate() {
  let weekEndDate = new Date(nowYear, nowMonth, nowDay + (6 - nowDayOfWeek));
  return formatDate(weekEndDate);
}
//获得上周的开端日期
function getBeforeWeekStartDate() {
  let weekStartDate = new Date(nowYear, nowMonth, nowDay - 7 - nowDayOfWeek);
  return formatDate(weekStartDate);
}
//获得上周的停止日期
function getBeforeWeekEndDate() {
  let weekEndDate = new Date(nowYear, nowMonth, nowDay + (6 - nowDayOfWeek - 7));
  return formatDate(weekEndDate);
}
//获得本月的开端日期
function getMonthStartDate() {
  let monthStartDate = new Date(nowYear, nowMonth, 1);
  return formatDate(monthStartDate);
}
//获得本月的停止日期
function getMonthEndDate() {
  let monthEndDate = new Date(nowYear, nowMonth, getMonthDays(nowMonth));
  return formatDate(monthEndDate);
}
//获得上月开端时候
function getLastMonthStartDate() {
  let lastMonthStartDate = new Date(new Date().getFullYear(), new Date().getMonth() - 1, 1)
  return formatDate(lastMonthStartDate);
}
//获得上月停止时候
function getLastMonthEndDate() {
  var date = new Date();
  var day = new Date(date.getFullYear(), date.getMonth(), 0).getDate();
  var lastMonthEndDate = new Date(new Date().getFullYear(), new Date().getMonth() - 1, day);
  return formatDate(lastMonthEndDate);
}
//获得某月的天数
function getMonthDays(myMonth){
    let monthStartDate = new Date(nowYear, myMonth, 1);
    let monthEndDate = new Date(nowYear, myMonth + 1, 1);
    let days = (monthEndDate - monthStartDate)/(1000 * 60 * 60 * 24);
    return days;
}
//字段拼接
function substrDataStr(data) {
  let strArr = [];
  strArr.push(data.substr(0, 4))
  strArr.push(data.substr(4, 2))
  strArr.push(data.substr(6, 2))
  return strArr
}
//格局化日期：yyyy-MM-dd
function formatDate(date) {
  let myyear = date.getFullYear();
  let mymonth = date.getMonth() + 1;
  let myweekday = date.getDate();
  if (mymonth < 10) {
    mymonth = "0" + mymonth;
  }
  if (myweekday < 10) {
    myweekday = "0" + myweekday;
  }
  return (myyear + "" + mymonth + "" + myweekday);
}
//获取今天之前的多少天的日期
function GetDateStr(AddDayCount) {
  let dd = new Date();
  dd.setDate(dd.getDate() + AddDayCount); //获取AddDayCount天后的日期
  let y = dd.getFullYear();
  let m = dd.getMonth() + 1; //获取当前月份的日期
  let d = dd.getDate();
  if (m < 10) {
    m = "0" + m;
  }
  if (d < 10) {
    d = "0" + d;
  }
  return y + "" + m + "" + d;
}

export default {
  getWeekStartDate,
  getWeekEndDate,
  getBeforeWeekStartDate,
  getBeforeWeekEndDate,
  getMonthStartDate,
  getMonthEndDate,
  getLastMonthStartDate,
  getLastMonthEndDate,
  substrDataStr,
  formatDate,
  GetDateStr
}

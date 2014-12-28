---
layout: post
title: Google Places API and JSON parsing in Android
date: 2012-09-10 19:42:20.000000000 +05:30
categories:
- Android
tags:
- Android
- Google API
- Google Places API
- JSON
status: publish
type: post
published: true
meta:
  _edit_last: '33499249'
  _wpas_done_facebook: '1'
  _wpas_done_twitter: '1'
  _thumbnail_id: '43'
  tagazine-media: a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";i:0;s:6:"author";s:8:"33499249";s:7:"blog_id";s:8:"33466656";s:9:"mod_stamp";s:19:"2012-09-10
    20:42:10";}
  _wpas_skip_1585586: '1'
  _wpas_skip_1585600: '1'
author:
  login: yuvislm
  email: yuv.slm@gmail.com
  display_name: Yuvaraja
  first_name: ''
  last_name: ''
---
<p>Firstly about the Places API.</p>
<p><strong>Google Places API:</strong></p>
<p>Four basic Place requests are available from the API:</p>
<ul>
<li><a href="https://developers.google.com/places/documentation/search"><strong>Place Searches</strong></a> return a list of nearby Places based on a user's location or search string.</li>
<li><a href="https://developers.google.com/places/documentation/details"><strong>Place Details</strong></a> requests return more detailed information about a specific Place, including user reviews.</li>
<li><a href="https://developers.google.com/places/documentation/actions"><strong>Place Actions</strong></a> allow you to supplement the data in Google's Places Database with data from your application. Place Actions allow you to schedule Events, weight Place rankings by check-in data, or add and remove Places.</li>
<li><a href="https://developers.google.com/places/documentation/autocomplete"><strong>Places Autocomplete</strong></a> can be used to provide autocomplete functionality for text-based geographic searches, by returning Places as you type.</li>
</ul>
<p><strong>Note: </strong>The Result can be got as a JSON file or XML file depending on your needs.here I will be getting the result as a JSON.</p>
<p>You can refer more about Places API <a href="https://developers.google.com/places/documentation/">here</a>.</p>
<p>Step by Step procedure for using the Places API:</p>
<p><strong>Step 1:</strong></p>
<p>Create a new Android Project and add the android.permission.INTERNET to the manifest.</p>
<p><strong>Step 2:</strong></p>
<p>Now we have to send a HTTP Get request to the Google servers and receive a JSON file.</p>
<p>For this Purpose we will be importing the following classes.</p>
{% highlight java %}
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.DefaultHttpClient;
import org.json.JSONArray;
import org.json.JSONObject;
{% endhighlight %}
<p><strong>Step 3:</strong></p>
<p>First we have to have the URL to which the request is being sent.</p>
{% highlight java %}
String requesturl="https://maps.googleapis.com/maps/api/place/search/json?radius=500&amp;sensor=false&amp;key=&lt;your API key&gt;&amp;location=-33.8670522,151.1957362";
{% endhighlight %}
<p>Create a HttpClient from which the request must be sent.</p>
{% highlight java %}
DefaultHttpClient client=new DefaultHttpClient();
{% endhighlight %}
<p>Then we must construct the Http Request which is of GET method.</p>
{% highlight java %}
HttpGet req=new HttpGet(requesturl);
{% endhighlight %}
<p>Then we must send the request to get a response.</p>
{% highlight java %}
HttpResponse res=client.execute(req);
{% endhighlight %}
<p><strong>Step 4:</strong></p>
<p>After receiving the response we must extract the Entity from the HttpResponse. Then the JSONObject must be constructed from it. Since the JSONObject Constructor accepts only String we must convert the Contents of the HttpResponse into a String.</p>
{% highlight java %}
HttpEntity jsonentity=res.getEntity();
InputStream in=jsonentity.getContent();
JSONObject jsonobj=new JSONObject(convertStreamToString(in))
{% endhighlight %}
<p>The procedure for converting Stream to String.</p>
{% highlight java %}
private String convertStreamToString(InputStream in) {
		// TODO Auto-generated method stub
		BufferedReader br=new BufferedReader(new InputStreamReader(in));
		StringBuilder jsonstr=new StringBuilder();
		String line;
		try {
			while((line=br.readLine())!=null)
			{
				String t=line+""\n";
				jsonstr.append(t);
			}
			br.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
		return jsonstr.toString();
{% endhighlight %}

<p><strong>Step 5:</strong></p>
<p>Now is the time for Parsing.</p>
<p>The JSON returned is like this.</p>
{% highlight text %}
   "html_attributions" : [],
   "results" : [
      {
         "geometry" : {
            "location" : {
               "lat" : -33.86682180,
               "lng" : 151.19542210
            }
         },
         "icon" : "http://maps.gstatic.com/mapfiles/place_api/icons/geocode-71.png",
         "id" : "96760d4544ecdaaf2e87565915638238ca960f20",
         "name" : "Pirrama Rd",
         "reference" : "CoQBcgAAAIWIC2AA6D0V4r5BAzZ_-hdqlmDQO0FBGRNU4SGWhIv5a1yycXYi9d4w7K6-JeaQU_ZWrbXW19RPgF6VN_5iO05BeDof1DNzeBpsoSvIvFwrjkzZMPelEPVijJDxdu7f1Dr3BgPEvnxwUJ5eWO64rL9UGKMlicOdB5GMrc4cJ-3REhAVrcb67mrvPduZh4R80gPAGhQH0w5Yjv0k32AdYsbJb2_ycX2Kug",
         "types" : [ "route" ],
         "vicinity" : "Pyrmont"
      }
   ]
{% endhighlight %}
<p>So the Parser must be written to parse the JSON of above format.</p>
{% highlight java %}
JSONArray resarray=jsonobj.getJSONArray("results");
if(resarray.length()==0){
	//No Results
}
else{
        int len=resarray.length();
	for(int j=0;j&lt;len;j++)
	{
		Toast.makeText(getApplicationContext(), resarray.getJSONObject(j).getString("name") , Toast.LENGTH_LONG).show();
	}
{% endhighlight %}
<p>A Toast message is shown to the User about the returned place names.</p>


<div align="center"> <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=446907996" /> </div>
<div align="center"> <img src="https://github-readme-streak-stats.herokuapp.com/?user=446907996" /> </div>
<div align="center"> <img src="https://github-readme-activity-graph.vercel.app/graph?username=446907996&theme=xcode" /> </div>
:return: A svg badge with latest visitor count
"""

req_source = identity_request_source()

if not req_source:
    return invalid_count_resp('Missing required param: page_id')

latest_count = update_counter(req_source)

if not latest_count:
    return invalid_count_resp("Count API Failed")

# get left color and right color
left_color = "#595959"
if request.args.get("left_color") is not None:
    left_color = request.args.get("left_color")

right_color = "#1283c3"
if request.args.get("right_color") is not None:
    right_color = request.args.get("right_color")

left_text = "visitors"
if request.args.get("left_text") is not None:
    left_text = request.args.get("left_text")

svg = badge(left_text=left_text, right_text=str(latest_count),
            left_color=str(left_color), right_color=str(right_color))

expiry_time = datetime.datetime.utcnow() - datetime.timedelta(minutes=10)

headers = {'Cache-Control': 'no-cache,max-age=0,no-store,s-maxage=0,proxy-revalidate',
           'Expires': expiry_time.strftime("%a, %d %b %Y %H:%M:%S GMT")}

return Response(response=svg, content_type="image/svg+xml", headers=headers)

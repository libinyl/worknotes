Traceback (most recent call last):
  File "reporter_app_cnt_101.py", line 66, in <module>
    report_project(sys.argv[1], sys.argv[2])
  File "reporter_app_cnt_101.py", line 58, in report_project
    do_report(post_data)
  File "reporter_app_cnt_101.py", line 38, in do_report
    print("do_report|rsp|{}".format(res.text))
UnicodeEncodeError: 'ascii' codec can't encode characters in position 35-39: ordinal not in range(128)
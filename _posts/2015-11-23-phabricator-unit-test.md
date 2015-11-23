---
layout: post
title: Phabricator使用cpplint和gtest
---

>“工欲善其事，必先利其器”

Phabricator是facebook开源的一款优秀的code review工具。Phabricator支持配置lint和unit test工具。我们的代码主要使用c++，代码规范遵循google开源的c++代码规范。Phabricator本身不支持cpplint和gtest，需要自己配置。

## 配置使用cpplint

在git根目录加上.arclint文件，配置使用cpplint.py，内容如下
    {
        "linters": {
            "sample": {
                "type": "cpplint",
                "bin": ["cpplint.sh"],  # cpplint.sh封装了cpplint.py,增加了filters
                "include": "((\\.h$)|(\\.hpp$)|(\\.hh$)|(\\.cc$)|(\\.c$)|(\\.cpp$))"
            }
        }
    }

## 配置使用gtest
Phabricator只支持php的单元测试，如果需要使用gtest，需要自行配置。
在git根目录加上.arcconfig文件，配置使用自定义unit engine。
    {
        "unit.engine": "MyUnitTestEngine",
        "load" : [ "arc_tool/lib" ]
    }

在arc_tool/lib目录中新建__phutil_library_init__.php，内容如下:
    <?php
    phutil_register_library('my-unit-test-engine', __FILE__);
    ?>

新建一个unit engine，arc_tool/lib/engine/MyUnitTestEngine.php，内容如下：
    <?php

    final class MyUnitTestEngine extends ArcanistUnitTestEngine {

        public function run() {
            $sample_results = array();

            $ret = -1;
            $start_time = microtime(true);
            system("flame test ...", $ret);  # 我们使用flame工具编译代码和单元测试
            $end_time = microtime(true);

            if ($ret == 0) {
                $result_success = new ArcanistUnitTestResult();
                $result_success->setName("All test cases passed!");
                $result_success->setResult(ArcanistUnitTestResult::RESULT_PASS);
                $result_success->setDuration($end_time - $start_time);
                $sample_results[] = $result_success;
            } else {
                $result_failure = new ArcanistUnitTestResult();
                $result_failure->setName("A failed test");
                $result_failure->setResult(ArcanistUnitTestResult::RESULT_FAIL);
                $result_failure->setDuration($end_time - $start_time);
                $result_failure->setUserData($ret . " test cases failed!");
                $sample_results[] = $result_failure;
            }

            return $sample_results;
        }
    }
    ?>

执行下面的两个命令来生成arc工具使用到的类
    /var/phabricator/libphutil/scripts/phutil_rebuild_map.php arc_tool/lib
    arc liberate

## 测试

测试cpplint
    arc lint

测试gtest
    arc unit

发code review,自动会运行cpplint和gtest
    arc diff


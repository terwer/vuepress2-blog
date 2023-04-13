---
title: 友链💖
article: false
pageInfo: false
lastUpdated: false
editLink: false
prev: false
next: false
sidebar: false
toc: false
---

## 我和我的朋友们

<LinkLayout :linkList="linkList"/>


## 加入我们

<hr/>
<p>在下方评论区留言申请加入我的友链，请按如下格式提供信息：</p>
<ul>
    <li>博客名：浅海拾贝</li>
	<li>简介：寻找未知的技术拼图</li>
	<li>图片：https://terwer.space/logo.svg</li>
	<li>链接：https://terwer.space</li>
</ul>

<script>
export default {
        data() {
                return {
                        linkList: [
                                {
                                    name: "vuepress-theme-hope",
                                    description: "感谢强大的VuePress主题Hope",
                                    imgurl: "https://theme-hope.vuejs.press/logo.svg",
                                    href: "https://theme-hope.vuejs.press/zh/",
                                },
                                {
                                    name: "花诽语",
                                    description: "授人与鱼不如授人与渔",
                                    imgurl: "https://www.sakurafeiyu.top/logo.png",
                                    href: "https://sakurafeiyu.top",
                                },
                        ],
                };
        }
};
</script>

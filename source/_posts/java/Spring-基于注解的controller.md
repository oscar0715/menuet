---
title: Spring_基于注解的controller
tags:
  - Spring
categories:
  - Techonology
  - Spring
date: 2016-06-24 21:17:10
---
Spring支持强大的，基于注解的Controller
在HTTP服务时，Spring可以自动的将通过http传过来的参数映射到对应的类中去。
<!-- more -->

***

#### Get方法用 @ModelAttribute
如果是GET
那么用 @ModelAttribute标签，相应的参数对注入到标签后面的类中去！
@ModelAttribute后面紧跟着BindingResult类，这个类中会记录，注入的情况！
{% codeblock lang:java  %}
@SuppressWarnings("unchecked")
@RequestMapping(value = "/route/appgetroutes", method = RequestMethod.GET)
@ResponseBody 
public TTResult<List<FootprintsResponse>> getFootprintsByUid(
        @ModelAttribute @Valid TTGetFootprintsByUidRequest getFootprintsByUidRequest,
        BindingResult result) {
    if (result.hasErrors()) {
        return TTResult.getBadParameterError(result);
    }
    return tTFootprintReadService
            .getFootprintsByUid(getFootprintsByUidRequest.getUserId());
}

public class TTGetFootprintsByUidRequest {
    @NotNull   // @Valid 和这个对应
    Long userId;

    public Long getUserId() {
        return userId;
    }

    public void setUserId(Long userId) {
        this.userId = userId;
    }
}

{% endcodeblock %}

#### Post方法用@RequestBody
Post方法，就用@RequestBody代替@ModelAttribute，道理很简单，这里的参数是放在POST的body里面的，自然要到body里去取咯！
{% codeblock lang:java  %}
@RequestMapping(value = "/route/appaddroute", method = RequestMethod.POST)
@ResponseBody
public TTResult<AddFootprintResponse> addFootprint(
        @RequestBody @Valid TTAddFootprintRequest tTAddFootprintRequest,
        BindingResult result) {
    if (result.hasErrors()) {
        return TTResult.getBadParameterError(result);
    }
    return tTFootPrintWriteService.addFootprint(tTAddFootprintRequest);
}
{% endcodeblock %}

***

#### 参考
1. {% link Spring MVC 中的基于注解的 Controller http://zachary-guo.iteye.com/blog/1318597 Spring MVC 中的基于注解的 Controller %}
2. {% link Spring MVC 参数校验 http://www.cnblogs.com/eggbucket/p/3264074.html Spring MVC 参数校验 %}

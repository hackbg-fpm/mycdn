---
title: ' Java代码笔记'
tags:
  - Java
categories:
  - 笔记
summary: Java代码随记
img: 
abbrlink: c9a
date: 2022-03-15 13:00:00
updated: 2022-03-16 13:00:00
---



## 常规CRUD

### 实体类

~~~java
package com.psds.credit.entity;

import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;
import java.io.Serializable;
import java.util.Date;
import javax.persistence.*;
import lombok.Data;

/**
 * Table: T_XYYJ_YJLX
 * TableName: 信用预警——预警类型信息表
 */
@Data
@ApiModel(value="信用预警——预警类型信息表")
@Table(name = "T_XYYJ_YJLX")
public class WarningType implements Serializable {
    /**
     * 预警类型主键
     */
    @Id
    @Column(name = "YJLX_ID")
    @GeneratedValue(generator = "JDBC")
    @ApiModelProperty(value="预警类型主键")
    private Integer warningTypeId;
    /**
     * 预警类型编码
     */
    @Column(name = "YJLX_BM")
    @ApiModelProperty(value="预警类型编码")
    private String warningTypeCode;
    /**
     * 预警类型名称
     */
    @Column(name = "YJLX_MC")
    @ApiModelProperty(value="预警类型名称")
    private String warningTypeName;
    /**
     * 预警类型说明
     */
    @Column(name = "YJLX_SM")
    @ApiModelProperty(value="预警类型说明")
    private String warningTypeContext;
    /**
     * 创建时间
     */
    @Column(name = "YJLX_Create_Time")
    @ApiModelProperty(value="创建时间")
    private Date createTime;
    /**
     * 创建用户编号
     */
    @Column(name = "YJLX_Create_User_Id")
    @ApiModelProperty(value="创建用户编号")
    private Integer createUserId;
    /**
     * 创建用户名称
     */
    @Column(name = "YJLX_Create_User_Name")
    @ApiModelProperty(value="创建用户名称")
    private String createUserName;
    /**
     * 更新时间
     */
    @Column(name = "YJLX_Update_Time")
    @ApiModelProperty(value="更新时间")
    private Date updateTime;
    /**
     * 更新用户编号
     */
    @Column(name = "YJLX_Update_User_Id")
    @ApiModelProperty(value="更新用户编号")
    private Integer updateUserId;
    /**
     * 更新用户名称
     */
    @Column(name = "YJLX_Update_User_Name")
    @ApiModelProperty(value="更新用户名称")
    private String updateUserName;

    private static final long serialVersionUID = 1L;
}
~~~

### Mapper类

~~~java
package com.psds.credit.mapper;
import com.psds.credit.entity.WarningType;
import tk.mybatis.mapper.common.Mapper;
public interface WarningTypeMapper extends Mapper<WarningType> {
}
~~~

### Model类

~~~java
package com.psds.credit.model;
import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;
import lombok.Data;
import javax.validation.constraints.NotBlank;
import java.io.Serializable;

/**
 * @author ChenZhiMing
 * @Date 2022年1月19日, 0019
 */
@ApiModel("新增预警类型请求类")
@Data
public class WarningTypeAddBody implements Serializable {
    private static final long serialVersionUID = -7931205996167983238L;
    /**
     * 预警类型编码
     */
    @ApiModelProperty(value="预警类型编码")
    @NotBlank(message = "预警类型编码不能为空")
    private String warningTypeCode;
    /**
     * 预警类型名称
     */
    @ApiModelProperty(value="预警类型名称")
    @NotBlank(message = "预警类型名称不能为空")
    private String warningTypName;
    /**
     * 预警类型说明
     */
    @ApiModelProperty(value="预警类型说明")
    private String warningTypeContext;
}

~~~

~~~java
package com.psds.credit.model;
import com.psds.credit.framework.boot.model.BaseRequest;
import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;
import lombok.Data;

/**
 * @author ChenZhiMing
 * @Date 2022年1月19日, 0019
 */
@ApiModel("模糊查询预警类型请求类")
@Data
public class WarningTypeFindByKey extends BaseRequest {
    private static final long serialVersionUID = -3343948693858710848L;
    /**
     * 预警类型编码
     */
    @ApiModelProperty(value="预警类型编码")
    private String warningTypeCode;
    /**
     * 预警类型名称
     */
    @ApiModelProperty(value="预警类型名称")
    private String warningTypeName;
    /**
     * 预警类型说明
     */
    @ApiModelProperty(value="预警类型说明")
    private String warningTypeContext;
}

~~~

~~~java
package com.psds.credit.model;
import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;
import lombok.Data;
import javax.validation.constraints.NotBlank;
import java.io.Serializable;

/**
 * @author ChenZhiMing
 * @Date 2022年1月19日, 0019
 */
@ApiModel("预警类型修改请求类")
@Data
public class WarningTypeUpdBody implements Serializable {
    private static final long serialVersionUID = 7212177218485625148L;
    @ApiModelProperty(value="预警类型主键")
    private Integer warningTypeId;

    @ApiModelProperty(value="预警类型编码")
    @NotBlank(message = "预警类型编码不能为空")
    private String warningTypeCode;

    /**
     * 预警类型名称
     */
    @ApiModelProperty(value="预警类型名称")
    @NotBlank(message = "预警类型名称不能为空")
    private String warningTypeName;

    /**
     * 预警类型说明
     */
    @ApiModelProperty(value="预警类型说明")
    private String warningTypeContext;
}

~~~

### Service类

```java
package com.psds.credit.service;
import com.baomidou.dynamic.datasource.annotation.DS;
import com.github.pagehelper.PageHelper;
import com.github.pagehelper.PageInfo;
import com.psds.credit.entity.WarningType;
import com.psds.credit.mapper.WarningTypeMapper;
import com.psds.credit.model.WarningTypeFindByKey;
import org.apache.commons.lang3.StringUtils;
import org.springframework.stereotype.Service;
import tk.mybatis.mapper.entity.Example;
import javax.annotation.Resource;
import java.util.List;
import java.util.Optional;

/**
 * 预警类型管理
 * @author ChenZhiMing
 * @Date 2022年1月19日, 0019
 */
@Service
@DS("XYYJ")
public class WarningTypeService {
    @Resource
    private WarningTypeMapper warningTypeMapper;
    /**
     * 新增预警类型
     *
     * @param warningType:
     * @return
     */
    public int addWarningType(WarningType warningType) {
        return warningTypeMapper.insertSelective(warningType);
    }

    /**
     * 删除预警类型
     *
     * @param id:
     * @return
     */
    public int delWarningType(Integer id) {
        return warningTypeMapper.deleteByPrimaryKey(id);
    }

    /**
     * 修改预警类型
     *
     * @param warningType:
     * @return
     */
    public int updateWarningType(WarningType warningType) {
        return warningTypeMapper.updateByPrimaryKeySelective(warningType);
    }

    /**
     * 根据id查询预警类型
     *
     * @param id:
     * @return
     */
    public Optional<WarningType> findWarningTypeById(Integer id) {
        WarningType warningType = warningTypeMapper.selectByPrimaryKey(id);
        return Optional.ofNullable(warningType);
    }

    /**
     * 查询所有预警类型
     *
     * @param :
     * @return
     */
    public PageInfo<WarningType> getWarningTypeList(WarningTypeFindByKey req) {
        Example example = new Example(WarningType.class);
        Example.Criteria criteria = example.createCriteria();
        if (StringUtils.isNotBlank(req.getWarningTypeCode())){
            criteria.andLike("warningTypeCode", "%"+req.getWarningTypeCode()+"%");
        }
        if (StringUtils.isNotBlank(req.getWarningTypeName())) {
            criteria.andLike("warningTypeName", "%"+req.getWarningTypeName()+"%");
        }
        if (StringUtils.isNotBlank(req.getOrderField())) {
            //用户自定义排序
            Example.OrderBy orderBy = example.orderBy(req.getOrderField());
            if (StringUtils.isNotBlank(req.getOrderSort())) {
                if ("asc".equals(req.getOrderSort())) {
                    orderBy.asc();
                } else {
                    orderBy.desc();
                }
            }
        } else {
            //设置默认排序
            example.orderBy("warningTypeId").desc();
        }
        PageHelper.startPage(req.getPageNo(), req.getPageSize());
        List<WarningType> dataList = warningTypeMapper.selectByExample(example);
        PageInfo<WarningType> pageInfo = new PageInfo<>(dataList);
        return pageInfo;
    }
}

```

### Controller类

```java
package com.psds.credit.controller;
import com.alibaba.fastjson.JSONObject;
import com.github.pagehelper.PageInfo;
import com.psds.credit.common.core.controller.BaseController;
import com.psds.credit.entity.WarningType;
import com.psds.credit.framework.boot.message.CommonMessage;
import com.psds.credit.framework.boot.message.ResultData;
import com.psds.credit.model.WarningTypeAddBody;
import com.psds.credit.model.WarningTypeFindByKey;
import com.psds.credit.model.WarningTypeUpdBody;
import com.psds.credit.service.WarningTypeService;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.BeanUtils;
import org.springframework.web.bind.annotation.*;
import javax.annotation.Resource;
import javax.validation.Valid;
import java.util.*;

/**
 * @author ChenZhiMing
 * @Date 2022年1月19日, 0019
 */
@Slf4j
@RestController
@Api(tags = "预警类型管理")
@RequestMapping("warningType")
public class WarningTypeController extends BaseController {
    @Resource
    private WarningTypeService warningTypeService;
    /**
     * 新增预警类型
     *
     * @param warningTypeAddBody:
     * @return
     */
    @PostMapping("/addWarningType")
    @ApiOperation(value = "新增预警类型", notes = "新增预警类型")
    public ResultData<Boolean> addWarningType(@RequestBody @Valid WarningTypeAddBody warningTypeAddBody) {
        log.info("新增预警类型,请求参数:{}", JSONObject.toJSONString(warningTypeAddBody));
        ResultData<Boolean> resultData = new ResultData<>(CommonMessage.ERROR, "操作失败");
        resultData.setData(false);
        try {
            WarningType warningType = new WarningType();
            BeanUtils.copyProperties(warningTypeAddBody, warningType);
            warningType.setCreateTime(new Date());
            warningType.setCreateUserName(getUsername());
            warningType.setCreateUserId(Math.toIntExact(getUserId()));
            boolean bRtn = warningTypeService.addWarningType(warningType) > 0;
            if (bRtn) {
                resultData.setCode(CommonMessage.SUCCESS);
                resultData.setMsg("操作成功");
                resultData.setData(true);
            }
        } catch (Exception ex) {
            log.error("新增预警类型出现异常:{}", ex.getMessage(), ex);
            resultData.setCode(CommonMessage.EXCEPTION);
            resultData.setMsg("新增预警类型出现异常");
        }
        return resultData;
    }

    /**
     * 删除预警类型
     *
     * @param id:
     * @return
     */
    @PostMapping("/delWarningType/{id}")
    @ApiOperation(value = "删除预警类型", notes = "根据id删除预警类型")
    public ResultData<Boolean> delWarningType(@PathVariable Integer id) {
        log.info("删除预警类型,请求参数:{}", JSONObject.toJSONString(id));
        ResultData<Boolean> resultData = new ResultData<>(CommonMessage.ERROR, "操作失败");
        resultData.setData(false);
        try {
            boolean bRtn = warningTypeService.delWarningType(id) > 0;
            if (bRtn) {
                resultData.setCode(CommonMessage.SUCCESS);
                resultData.setMsg("操作成功");
                resultData.setData(true);
            }
        } catch (Exception ex) {
            log.error("删除预警类型出现异常:{}", ex.getMessage(), ex);
            resultData.setCode(CommonMessage.EXCEPTION);
            resultData.setMsg("删除预警类型出现异常");
        }
        return resultData;
    }

    /**
     * 修改预警类型
     *
     * @param warningTypeUpdBody:
     * @return
     */
    @PostMapping("/updWarningType")
    @ApiOperation(value = "修改预警类型", notes = "根据id修改预警类型")
    public ResultData<Boolean> updateWarningType(@RequestBody @Valid WarningTypeUpdBody warningTypeUpdBody) {
        log.info("修改预警类型,请求参数:{}", JSONObject.toJSONString(warningTypeUpdBody));
        ResultData<Boolean> resultData = new ResultData<>(CommonMessage.ERROR, "操作失败");
        resultData.setData(false);
        try {
            WarningType warningType = new WarningType();
            BeanUtils.copyProperties(warningTypeUpdBody, warningType);
            warningType.setUpdateTime(new Date());
            warningType.setUpdateUserName(getUsername());
            warningType.setUpdateUserId(Math.toIntExact(getUserId()));
            boolean bRtn = warningTypeService.updateWarningType(warningType) > 0;
            if (bRtn) {
                resultData.setCode(CommonMessage.SUCCESS);
                resultData.setMsg("修改成功");
                resultData.setData(true);
            }
        } catch (Exception ex) {
            log.error("修改预警类型出现异常:{}", ex.getMessage(), ex);
            resultData.setCode(CommonMessage.EXCEPTION);
            resultData.setMsg("修改预警类型出现异常");
        }
        return resultData;
    }

    /**
     * 根据id查询预警类型
     *
     * @param id:
     * @return
     */
    @PostMapping("/findWarningTypeById/{id}")
    @ApiOperation(value = "根据id查询预警类型", notes = "根据id查询预警类型")
    public ResultData<Optional<WarningType>> findWarningTypeById(@PathVariable Integer id) {
        log.info("根据id查询预警类型,请求参数:{}", JSONObject.toJSONString(id));
        ResultData<Optional<WarningType>> resultData = new ResultData<>(CommonMessage.ERROR, "操作失败");
        try {
            Optional<WarningType> warningTypeById = warningTypeService.findWarningTypeById(id);
            if (warningTypeById.isPresent()) {
                resultData.setCode(CommonMessage.SUCCESS);
                resultData.setMsg("根据id查询预警类型成功");
                resultData.setData(warningTypeById);
            } else {
                resultData.setCode(CommonMessage.ERROR);
                resultData.setMsg("根据id查询预警类型为空");
            }
        } catch (Exception ex) {
            log.error("根据id查询预警类型出现异常:{}", ex.getMessage(), ex);
            resultData.setCode(CommonMessage.EXCEPTION);
            resultData.setMsg("根据id查询预警类型出现异常");
        }
        return resultData;
    }

    /**
     * 查询所有预警类型
     *
     * @param :
     * @return
     */
    @PostMapping("/getWarningTypeList")
    @ApiOperation(value = "条件查询预警类型", notes = "按条件查询预警类型,默认排序：主键")
    public ResultData<List<WarningType>> getWarningTypeList(@RequestBody WarningTypeFindByKey baseRequest) {
        log.info("条件查询预警类型, 请求参数:{}", JSONObject.toJSONString(baseRequest));
        ResultData<List<WarningType>> resultData = new ResultData<>(CommonMessage.ERROR, "操作失败");
        try {
            PageInfo<WarningType> pageInfo = warningTypeService.getWarningTypeList(baseRequest);
            resultData.setCode(CommonMessage.SUCCESS);
            resultData.setMsg("操作成功");
            resultData.setData(pageInfo.getList());
            Map<String, Object> other = new HashMap<>();
            //获取总记录数
            other.put("total", pageInfo.getTotal());
            //获取总页数
            other.put("pages", pageInfo.getPages());
            resultData.setOther(other);
        } catch (Exception ex) {
            log.error("条件查询预警类型出现异常:{}", ex.getMessage(), ex);
            resultData.setCode(CommonMessage.EXCEPTION);
            resultData.setMsg("条件查询预警类型出现异常");
        }
        return resultData;
    }
}

```

----



## 多表操作Service（一对多的主从表）

### 新增

~~~java
@Transactional(rollbackFor = Exception.class)
    public int addTCustomReport(@Valid TCustomReportAdd customReportAdd) {
        int j = 0;
        TCustomReport tCustomReport = new TCustomReport();
        BeanUtils.copyProperties(customReportAdd, tCustomReport);
        tCustomReport.setReportCreateTime(new Date());
        IJWTInfo jwtInfo = (IJWTInfo) request.getAttribute("JWTInfo");
        tCustomReport.setReportCreateUserId(jwtInfo.getId());
        tCustomReport.setReportCreateUserName(jwtInfo.getUniqueName());
        int i = tCustomReportMapper.insertSelective(tCustomReport);
        if (i > 0) {
            List<TCustomReportDetailAdd> tCustomReportAddList = customReportAdd.getTCustomReportDetailAdds();
            for (TCustomReportDetailAdd tCustomReportDetailAdd : tCustomReportAddList) {
                TCustomReportDetail tCustomReportDetail = new TCustomReportDetail();
                BeanUtils.copyProperties(tCustomReportDetailAdd, tCustomReportDetail);
                tCustomReportDetail.setReportId(tCustomReport.getReportId());
                j = tCustomReportDetailMapper.insertSelective(tCustomReportDetail);
            }
        }
        return j;
    }
~~~

### 修改

~~~java
 @Transactional(rollbackFor = Exception.class)
    public int updateTCustomReport(@Valid TCustomReportUpddBody tCustomReportUpdBody) {
        int j = 0;
        TCustomReport tCustomReport = new TCustomReport();
        BeanUtils.copyProperties(tCustomReportUpdBody, tCustomReport);
        tCustomReport.setReportUpdateTime(new Date());
        IJWTInfo jwtInfo = (IJWTInfo) request.getAttribute("JWTInfo");
        tCustomReport.setReportUpdateUserId(jwtInfo.getId());
        tCustomReport.setReportUpdateUserName(jwtInfo.getUniqueName());
        int i = tCustomReportMapper.updateByPrimaryKeySelective(tCustomReport);
        if (i > 0) {
            //先根据主表id删除子数据
            Example exampleDel = new Example(TCustomReportDetail.class);
            exampleDel.createCriteria().andEqualTo("reportId",tCustomReportUpdBody.getReportId());
            tCustomReportDetailMapper.deleteByExample(exampleDel);
            //插入要修改的数据
            List<TCustomReportDetailAdd> tCustomReportUpdList = tCustomReportUpdBody.getTCustomReportDetailUpd();
            for (TCustomReportDetailAdd tCustomReportDetailUpd : tCustomReportUpdList) {
                TCustomReportDetail tCustomReportDetail = new TCustomReportDetail();
                BeanUtils.copyProperties(tCustomReportDetailUpd, tCustomReportDetail);
                tCustomReportDetail.setReportId(tCustomReportUpdBody.getReportId());
                j = tCustomReportDetailMapper.insertSelective(tCustomReportDetail);
            }
        }
        return j;
    }
~~~

### 根据id查询

```java
 public TCustomReportFindByIdBody findTCustomReportById(Integer id) {
        TCustomReport tCustomReport = tCustomReportMapper.selectByPrimaryKey(id);
        TCustomReportFindByIdBody tCustomReportFindByIdBody = new TCustomReportFindByIdBody();
        if (tCustomReport != null) {
            BeanUtils.copyProperties(tCustomReport, tCustomReportFindByIdBody);
            Example example = new Example(TCustomReportDetail.class);
            example.createCriteria().andEqualTo("reportId", tCustomReport.getReportId());
            List<TCustomReportDetail> tCustomReportDetails = tCustomReportDetailMapper.selectByExample(example);
            tCustomReportFindByIdBody.setTCustomReportDetail(tCustomReportDetails);
        }
        return tCustomReportFindByIdBody;
    }
```

### 模糊查询并分页

​        先查询主表，再新建一个与主表一样大小的用于分页的model（包含主从表所需的属性）列表，实例化这个列表，将主表的信息拷贝进model，在将从表的信息拷贝进model。

```java
public PageInfo<TCustomReportFindByIdBody> getTCustomReportList(TCustomReportFindByKey req) {
    Example example = new Example(TCustomReport.class);
    Example.Criteria criteria = example.createCriteria();
    if (req.getReportStatus() != null) {
        criteria.andLike("reportStatus", "%" + req.getReportStatus() + "%");
    }
    if (StringUtils.isNotBlank(req.getReportName())) {
        criteria.andLike("reportName", "%" + req.getReportName() + "%");
    }
    if (StringUtils.isNotBlank(req.getReportCode())) {
        criteria.andLike("reportCode", "%" + req.getReportCode() + "%");
    }
    if (StringUtils.isNotBlank(req.getReportUserName())) {
        criteria.andLike("reportUserName", "%" + req.getReportUserName() + "%");
    }
    if (StringUtils.isNotBlank(req.getReportUserId())) {
        criteria.andLike("reportUserId", "%" + req.getReportUserId() + "%");
    }
    if (StringUtils.isNotBlank(req.getOrderField())) {
        //用户自定义排序
        Example.OrderBy orderBy = example.orderBy(req.getOrderField());
        if (StringUtils.isNotBlank(req.getOrderSort())) {
            if ("asc".equals(req.getOrderSort())) {
                orderBy.asc();
            } else {
                orderBy.desc();
            }
        }
    } else {
        //设置默认排序
        example.orderBy("reportId").desc();
    }
    PageHelper.startPage(req.getPageNo(), req.getPageSize());
    List<TCustomReport> dataList = tCustomReportMapper.selectByExample(example);
    PageInfo<TCustomReport> pageInfoTCustomReport = new PageInfo<>(dataList);
    //创建和主表List一样长度的主从表List
    List<TCustomReportFindByIdBody> tCustomReportFindByIdBody = Arrays.asList(new TCustomReportFindByIdBody[dataList.size()]);
    //返回分页主从表
    for (int i = 0; i < dataList.size(); i++) {
        //实例化list
        tCustomReportFindByIdBody.set(i,new TCustomReportFindByIdBody());
        if (dataList.get(i) != null) {
            BeanUtils.copyProperties(dataList.get(i), tCustomReportFindByIdBody.get(i));
            Example exampleDetail = new Example(TCustomReportDetail.class);
            exampleDetail.createCriteria().andEqualTo("reportId", dataList.get(i).getReportId());
            List<TCustomReportDetail> tCustomReportDetails = tCustomReportDetailMapper.selectByExample(exampleDetail);
            if (tCustomReportDetails != null && tCustomReportFindByIdBody.get(i)!=null) {
              tCustomReportFindByIdBody.get(i).setTCustomReportDetail(tCustomReportDetails);
            }
        }
    }
    PageInfo<TCustomReportFindByIdBody> pageInfo = new PageInfo<>(tCustomReportFindByIdBody);
    pageInfo.setTotal(pageInfoTCustomReport.getTotal());
    pageInfo.setPages(pageInfoTCustomReport.getPages());
    return pageInfo;
}

```

❤❤注意tCustomReportFindByIdBody.set(i,new TCustomReportFindByIdBody());

----



## 批量导入

### 例1

#### Model类

```java
package com.psds.credit.model.device.parts;

import com.psds.credit.common.annotation.Excel;
import lombok.Data;
import java.io.Serializable;

/**
 * 批量入库模板类
 * @author ChenZhiMing
 * @Date 2021年11月18日, 0018
 */
@Data
public class DeviceImportStorage implements Serializable {
    private static final long serialVersionUID = 72977236488394554L;
    /**
     * 备件名称
     */
    @Excel(name="备件名称")
    private String partsInfoName;
    /**
     * 入库单号
     */
    @Excel(name="采购单号")
    private String storageNum;
    /**
     * 仓库名称
     */
    @Excel(name="入库仓库")
    private String storageWarehouse;
    /**
     * 入库数量
     */
    @Excel(name="入库数量")
    private Integer storageCount;
}

```

#### Service类

```java
    /**
     * 批量导入入库信息
     *
     * @param partsList: 备件数据列表
     * @return
     */
    @Transactional(rollbackFor = Exception.class)
	public String importPartsStorage(List<DeviceImportStorage> partsList,  String sysUser) {
        if (com.psds.credit.common.utils.StringUtils.isNull(partsList) || partsList.size() == 0) {
            throw new ServiceException("导入入库数据不能为空！");
        }
        int successNum = 0;
        int failureNum = 0;
        StringBuilder successMsg = new StringBuilder();
        StringBuilder failureMsg = new StringBuilder();
        for (DeviceImportStorage parts : partsList) {
            try {
                DevicePartsStorage partsStorage=new DevicePartsStorage();
                partsStorage.setStorageWarehouse(parts.getStorageWarehouse());
                partsStorage.setStorageNum(parts.getStorageNum());
                partsStorage.setStorageCount(parts.getStorageCount());
                partsStorage.setCreateBy(sysUser);
                partsStorage.setCreateTime(new Date());
                Example example = new Example(DeviceParts.class);
                example.createCriteria().andEqualTo("partsInfoName",parts.getPartsInfoName());
                List<DeviceParts> pageInfoList=devicePartsMapper.selectByExample(example);
                partsStorage.setPartsInfoId(pageInfoList.get(0).getPartsInfoId());
                devicePartsStorageMapper.insertSelective(partsStorage);
                successNum++;
                successMsg.append("<br/>" + successNum + "、入库信息 " + parts.getPartsInfoName() + " 导入成功");
                //更新仓库备件计数表对应的备件数量
                //1判断对应的id和仓库是否存在
                Example example3 = new Example(DevicePartsCount.class);
                Example.Criteria criteria3 = example3.createCriteria();
                if (StringUtils.isNotBlank(partsStorage.getPartsInfoId().toString())) {
                    criteria3.andEqualTo("countWarehouse", parts.getStorageWarehouse());
                    criteria3.andEqualTo("partsInfoId", partsStorage.getPartsInfoId());
                }
                List<DevicePartsCount> i = devicePartsCountMapper.selectByExample(example3);
                if (i.size() > 0) {
                    //是，修改数量
                    int count2 = i.get(0).getCountSum();
                    Example example4 = new Example(DevicePartsCount.class);
                    Example.Criteria criteria2 = example4.createCriteria();
                    criteria2.andEqualTo("countWarehouse", parts.getStorageWarehouse());
                    criteria2.andEqualTo("partsInfoId", partsStorage.getPartsInfoId());
                    DevicePartsCount dc = new DevicePartsCount();
                    dc.setCountSum(count2 + parts.getStorageCount());
                    devicePartsCountMapper.updateByExampleSelective(dc, example4);
                } else {
                    //否，插入数据
                    DevicePartsCount dc = new DevicePartsCount();
                    dc.setCountSum(parts.getStorageCount());
                    dc.setCountWarehouse(parts.getStorageWarehouse());
                    dc.setPartsInfoId(partsStorage.getPartsInfoId());
                    devicePartsCountMapper.insertSelective(dc);
                }
                //修改总库存
                //查询仓库配置表对应仓库备件数量
                Example configCountE = new Example(DevicePartsWarehouseConfig.class);
                if (StringUtils.isNotBlank(parts.getStorageWarehouse())) {
                    configCountE.createCriteria().andEqualTo("warehouseConfigName", parts.getStorageWarehouse());
                }
                List<DevicePartsWarehouseConfig> s = warehouseConfigMapper.selectByExample(configCountE);
                int sum = s.get(0).getWarehouseConfigNum();
                DevicePartsWarehouseConfig warehouseConfig = new DevicePartsWarehouseConfig();
                warehouseConfig.setWarehouseConfigNum(sum + parts.getStorageCount());
                Example configExample = new Example(DevicePartsWarehouseConfig.class);
                configExample.createCriteria().andEqualTo("warehouseConfigName", parts.getStorageWarehouse());
                warehouseConfigMapper.updateByExampleSelective(warehouseConfig, configExample);
            } catch (Exception e) {
                failureNum++;
                String msg = "<br/>" + failureNum + "、入库信息 " + parts.getPartsInfoName() + " 导入失败：";
                failureMsg.append(msg + e.getMessage());
                log.error(msg, e);
            }
        }
        if (failureNum > 0) {
            failureMsg.insert(0, "很抱歉，导入失败！共 " + failureNum + " 条数据格式不正确，错误如下：");
            throw new ServiceException(failureMsg.toString());
        } else {
            successMsg.insert(0, "恭喜您，数据已全部导入成功！共 " + successNum + " 条，数据如下：");
        }
        return successMsg.toString();
    }
```

#### Controller类

```java
 /**
     *  批量导入入库信息
     * @param file: 文件
     *
     * @return
     */
    @ApiOperation(value = "批量导入入库信息",notes = "")
    @PostMapping("/importPartsStorage")
    public AjaxResult importPartsStorage(@RequestPart("file") MultipartFile file) throws Exception {
        ExcelUtil<DeviceImportStorage> util = new ExcelUtil<DeviceImportStorage>(DeviceImportStorage.class);
        List<DeviceImportStorage> partsList = util.importExcel(file.getInputStream());
        String sysUser = getUsername();
        String message = devicePartsStorageService.importPartsStorage(partsList, sysUser);
        return AjaxResult.success(message);
    }

    /**
     *  导出备件入库模板
     * @param :
     * @return
     */
    @GetMapping("/importStorageTemplate")
    @ApiOperation(value = "导出备件入库模板", notes = "导出备件入库批量导入的Excel模板")
    public AjaxResult importTemplate() {
        ExcelUtil<DeviceImportStorage> util = new ExcelUtil<DeviceImportStorage>(DeviceImportStorage.class);
        return util.importTemplateExcel("备件入库数据");
    }
```

#### Excel转换


![](/images/pasted-1.png)


![](/images/pasted-2.png)

### 例2

#### Model类

```java
package com.psds.credit.model.xygl;

import com.fasterxml.jackson.annotation.JsonFormat;
import com.psds.credit.common.annotation.Excel;
import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;
import lombok.Data;

import javax.persistence.Column;
import java.io.Serializable;
import java.util.Date;

/**
 * @author ChenZhiMing
 * @Date 2022年3月4日, 0004
 */
@ApiModel("活动导入模板类")
@Data
public class ImportActivityReq implements Serializable {
    private static final long serialVersionUID = -3253903642039788589L;
  
    @ApiModelProperty(value="信易行活动名称")
    @Excel(name = "活动名称")
    private String activityName;

    @ApiModelProperty(value="信易行活动举办单位")
    @Excel(name = "活动举办单位")
    private String activityHost;

   
    @ApiModelProperty(value="信易行活动开始时间")
    @Excel(name = "活动开始时间")
    @JsonFormat(pattern = "yyyy-MM-dd",timezone = "GMT+8")
    private Date activityStartTime;

   
    @ApiModelProperty(value="信易行活动结束时间")
    @Excel(name = "活动结束时间")
    @JsonFormat(pattern = "yyyy-MM-dd",timezone = "GMT+8")
    private Date activityEndTime;

   
    @ApiModelProperty(value="信易行活动描述")
    @Excel(name = "活动描述")
    private String activityDescribe;

   
    @Excel(name = "活动状态(0:启用，1:禁用)")
    @ApiModelProperty(value="活动状态(0:启用，1:禁用)")
    private String activityState;
}

```

#### Service类

```java
 /**
     * 批量导入
     *
     * @param is:
     * @param userId:
     * @param username:
     * @return
     */
    public Boolean importExcel(InputStream is, Integer userId, String username) {
        ExcelUtil<ImportActivityReq> excelUtil = new ExcelUtil<>(ImportActivityReq.class);
        try {
            List<ImportActivityReq> xingActivitys = excelUtil.importExcel(is);
            if (xingActivitys == null || xingActivitys.size() == 0) {
                throw new ServiceException("excel文件内容为空", 206);
            }
            xingActivitys.forEach(x -> {
                XINGActivity xingActivity = new XINGActivity();
                xingActivity.setXingActivityName(x.getActivityName());
                xingActivity.setXingActivityHost(x.getActivityHost());
                xingActivity.setXingActivityState(x.getActivityState());
                xingActivity.setXingActivityStartTime(x.getActivityStartTime());
                xingActivity.setXingActivityEndTime(x.getActivityEndTime());
                xingActivity.setXingActivityDescribe(x.getActivityDescribe());
                xingActivity.setDelFlag("0");
                xingActivity.setCreateTime(new Date());
                xingActivity.setCreateUserId(userId);
                xingActivity.setCreateUserName(username);
                xingActivityMapper.insertSelective(xingActivity);
            });
        } catch (Exception e) {
            e.printStackTrace();
            throw new ServiceException("文件导入失败或者excel文件内容不能为空", 206);
        }
        return true;
    }
```

#### Controller

```java
 /**
     *  批量导入
     * @param file: 
     * @return 
     */
    @ApiOperation("导入活动信息")
    @PostMapping("importExcel")
    public ResultData<Boolean> importExcel(@RequestPart("file")MultipartFile file){
        InputStream inputStream;
        try {
            inputStream = file.getInputStream();
        } catch (IOException e) {
            e.printStackTrace();
            throw new ServiceException("上传文件异常！", HttpStatus.NO_CONTENT);
        }
        Boolean aBoolean = xingActivityService.importExcel(inputStream,getUserId().intValue(),getUsername());
        ResultData<Boolean> resultData = new ResultData<>();
        resultData.setData(aBoolean);
        return resultData;
    }
```

### 例3

#### Model类

```java
@ApiModel("活动导入模板类")
@Data
public class ImportActivityReq implements Serializable {
    private static final long serialVersionUID = -3253903642039788589L;
    @Column(name = "XING_Activity_name")
    @ApiModelProperty(value="信易行活动名称")
    @Excel(name = "活动名称")
    private String activityName;

    /**
     * 信易行活动举办单位
     *
     * Table:     XYGL_XING_Activity
     * Column:    XING_Activity_host
     * Nullable:  true
     */
    @Column(name = "XING_Activity_host")
    @ApiModelProperty(value="信易行活动举办单位")
    @Excel(name = "活动举办单位")
    private String activityHost;

    /**
     * 信易行活动开始时间
     *
     * Table:     XYGL_XING_Activity
     * Column:    XING_Activity_start_time
     * Nullable:  true
     */
    @Column(name = "XING_Activity_start_time")
    @ApiModelProperty(value="信易行活动开始时间")
    @Excel(name = "活动开始时间")
    @JsonFormat(pattern = "yyyy-MM-dd",timezone = "GMT+8")
    private Date activityStartTime;

    /**
     * 信易行活动结束时间
     *
     * Table:     XYGL_XING_Activity
     * Column:    XING_Activity_end_time
     * Nullable:  true
     */
    @Column(name = "XING_Activity_end_time")
    @ApiModelProperty(value="信易行活动结束时间")
    @Excel(name = "活动结束时间")
    @JsonFormat(pattern = "yyyy-MM-dd",timezone = "GMT+8")
    private Date activityEndTime;

    /**
     * 信易行活动描述
     *
     * Table:     XYGL_XING_Activity
     * Column:    XING_Activity_describe
     * Nullable:  true
     */
    @Column(name = "XING_Activity_describe")
    @ApiModelProperty(value="信易行活动描述")
    @Excel(name = "活动描述")
    private String activityDescribe;

    /**
     * 信易行活动状态(0:启用，1:禁用)
     *
     * Table:     XYGL_XING_Activity
     * Column:    XING_Activity_state
     * Nullable:  true
     */
    @Column(name = "XING_Activity_state")
    @Excel(name = "活动状态(0:启用，1:禁用)")
    @ApiModelProperty(value="活动状态(0:启用，1:禁用)")
    private String activityState;
}
```

#### Service类

```java
public String importActivity(List<ImportActivityReq> activityList, Integer userId, String username) {
        if (com.psds.credit.common.utils.StringUtils.isNull(activityList) || activityList.size() == 0) {
            throw new ServiceException("导入活动信息不能为空！");
        }
        int successNum = 0;
        int failureNum = 0;
        StringBuilder successMsg = new StringBuilder();
        StringBuilder failureMsg = new StringBuilder();
        for (ImportActivityReq activity : activityList) {
            try {
                XINGActivity xingActivity = new XINGActivity();
                xingActivity.setXingActivityName(activity.getActivityName());
                xingActivity.setXingActivityHost(activity.getActivityHost());
                xingActivity.setXingActivityState(activity.getActivityState());
                xingActivity.setXingActivityStartTime(activity.getActivityStartTime());
                xingActivity.setXingActivityEndTime(activity.getActivityEndTime());
                xingActivity.setXingActivityDescribe(activity.getActivityDescribe());
                xingActivity.setDelFlag("0");
                xingActivity.setCreateTime(new Date());
                xingActivity.setCreateUserId(userId);
                xingActivity.setCreateUserName(username);
                xingActivityMapper.insertSelective(xingActivity);
                successNum++;
                successMsg.append("<br/>" + successNum + "、活动信息 " + activity.getActivityName() + " 导入成功");
            } catch (Exception e) {
                failureNum++;
                String msg = "<br/>" + failureNum + "、活动信息 " + activity.getActivityName() + " 导入失败：";
                failureMsg.append(msg + e.getMessage());
            }
        }
        if (failureNum > 0) {
            failureMsg.insert(0, "很抱歉，导入失败！共 " + failureNum + " 条数据格式不正确，错误如下：");
            throw new ServiceException(failureMsg.toString());
        } else {
            successMsg.insert(0, "恭喜您，数据已全部导入成功！共 " + successNum + " 条，数据如下：");
        }
        return successMsg.toString();
    }
```

#### Controller类

```java
    @ApiOperation(value = "导入活动信息",notes = "")
    @PostMapping("/importExcel")
    public ResultData importActivity(@RequestPart("file") MultipartFile file) throws Exception {
        ExcelUtil<ImportActivityReq> util = new ExcelUtil<ImportActivityReq>(ImportActivityReq.class);
        List<ImportActivityReq> activityList = util.importExcel(file.getInputStream());
        String message = xingActivityService.importActivity(activityList,Math.toIntExact(getUserId()),getUsername());
        ResultData resultData = new ResultData<>();
        resultData.setMsg(message);
        return resultData;
    }
```

## 导出

```java
 @ApiOperation("重点人员健康检测-导出数据")
    @PostMapping("export")
    public void export(@RequestBody MonitorPersonListVo monitorPersonListVo,
                             HttpServletRequest request, HttpServletResponse response) throws IOException {
        LoginUser loginUser = tokenService.getLoginUser(request);
        monitorPersonListVo.setDeptId(loginUser.getDeptId());
        List<MonitorPersonExportVo> list = monitorPersonService.selectExportList(monitorPersonListVo);
        ExcelUtil<MonitorPersonExportVo> util = new ExcelUtil<MonitorPersonExportVo>(MonitorPersonExportVo.class);
        util.exportExcel(response,list, "重点人员健康检测数据");
    }
```

### ExcelExportUtil

```java
package com.psds.credit.admin.config;

import com.alibaba.fastjson.JSONObject;
import com.psds.credit.admin.entity.*;
import org.apache.commons.lang3.StringUtils;
import org.apache.poi.hssf.usermodel.*;
import org.apache.poi.hssf.util.HSSFColor;
import org.apache.poi.ss.usermodel.*;
import org.apache.poi.xssf.usermodel.*;

import javax.servlet.ServletOutputStream;
import javax.servlet.http.HttpServletResponse;
import java.io.*;
import java.net.URLEncoder;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
import java.util.Map;

public class ExcelExportUtil {

    /**
     * 导出Excel
     *
     * @param sheetName       表格 sheet 的名称
     * @param headers         标题名称
     * @param dataList        需要显示的数据集合
     * @param exportExcelName 导出excel文件的名字
     */
    public void exportExcel(String sheetName, List<Map<String, Object>> dataList,
                            String[] headers, String exportExcelName) {

        // 声明一个工作薄
        XSSFWorkbook workbook = new XSSFWorkbook();
        // 生成一个表格
        XSSFSheet sheet = workbook.createSheet(sheetName);
        // 设置表格默认列宽度为15个字节
        sheet.setDefaultColumnWidth(15);

        // 生成表格中非标题栏的样式
        XSSFCellStyle style = workbook.createCellStyle();
        // 设置这些样式
        style.setFillForegroundColor(HSSFColor.HSSFColorPredefined.WHITE.getIndex());//背景色
        style.setFillPattern(FillPatternType.SOLID_FOREGROUND);
        style.setBorderBottom(BorderStyle.THIN);
        style.setBorderLeft(BorderStyle.THIN);
        style.setBorderRight(BorderStyle.THIN);
        style.setBorderTop(BorderStyle.THIN);
        style.setAlignment(HorizontalAlignment.CENTER);
        // 生成表格中非标题栏的字体
        XSSFFont font = workbook.createFont();
        font.setColor(HSSFColor.HSSFColorPredefined.BLACK.getIndex());
        font.setFontHeightInPoints((short) 12);
        font.setBold(true);
        // 把字体应用到当前的样式
        style.setFont(font);


        // 设置表格标题栏的样式
        XSSFCellStyle titleStyle = workbook.createCellStyle();
        titleStyle.setFillForegroundColor(HSSFColor.HSSFColorPredefined.BLUE_GREY.getIndex());
        titleStyle.setFillPattern(FillPatternType.SOLID_FOREGROUND);
        titleStyle.setBorderBottom(BorderStyle.THIN);
        titleStyle.setBorderLeft(BorderStyle.THIN);
        titleStyle.setBorderRight(BorderStyle.THIN);
        titleStyle.setBorderTop(BorderStyle.THIN);
        titleStyle.setAlignment(HorizontalAlignment.CENTER);
        titleStyle.setVerticalAlignment(VerticalAlignment.CENTER);
        // 设置标题栏字体
        XSSFFont titleFont = workbook.createFont();
        titleFont.setColor(HSSFColor.HSSFColorPredefined.WHITE.getIndex());
        titleFont.setFontHeightInPoints((short) 12);
        titleFont.setBold(true);
        // 把字体应用到当前的样式
        titleStyle.setFont(titleFont);

        // 产生表格标题行
        XSSFRow row = sheet.createRow(0);
        for (short i = 0; i < headers.length; i++) {
            XSSFCell cell = row.createCell(i);
            cell.setCellStyle(titleStyle);
            XSSFRichTextString text = new XSSFRichTextString(headers[i]);
            cell.setCellValue(text);
        }

        // 遍历集合数据，产生数据行
        Iterator<Map<String, Object>> it = dataList.iterator();
        int index = 0;
        while (it.hasNext()) {
            index++;
            row = sheet.createRow(index);
            Map<String, Object> data = it.next();
            int i = 0;
            for (String key : data.keySet()) {
                XSSFCell cell = row.createCell(i);
                cell.setCellStyle(style);
                XSSFRichTextString text = new XSSFRichTextString(data.get(key) + "");
                cell.setCellValue(text);
                i++;
            }
        }
        OutputStream out = null;
        try {
            String tmpPath = "C:\\excel\\" + exportExcelName + ".xlsx";
            out = new FileOutputStream(tmpPath);
            workbook.write(out);
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (workbook != null) {
                try {
                    workbook.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (out != null) {
                try {
                    out.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    /**
     * 创建每个 sheet 页的数据
     */
    private void createSheetData(HSSFWorkbook aWorkbook, String[] aTitles, String aSheetName,
                                 List<String[]> aRowData) {
        HSSFSheet tmpSheet = aWorkbook.createSheet(aSheetName);
        // 设置sheet的标题
        HSSFRow tmpTitileRow = tmpSheet.createRow(0);
        for (int i = 0; i < aTitles.length; i++) {
            tmpTitileRow.createCell(i).setCellValue(aTitles[i]);
        }
        // 遍历填充每行的数据
        HSSFRow tmpRow = null;
        int tmpRowNumber = 1;
        for (String[] rowData : aRowData) {
            tmpRow = tmpSheet.createRow(tmpRowNumber);
            for (int i = 0; i < rowData.length; i++) {
                tmpRow.createCell(i).setCellValue(rowData[i]);
            }
            tmpRowNumber++;
        }
    }

    /**
     * 设置响应头
     */
    private void setResponseHeader(HttpServletResponse aResponse, String aFileName) {
        try {
            try {
                aFileName = new String(aFileName.getBytes(), "UTF-8");
            } catch (UnsupportedEncodingException e) {
                e.printStackTrace();
            }
            aResponse.setContentType("application/octet-stream;charset=UTF-8");
            aResponse.setHeader("Content-Disposition", "attachment;filename=" + aFileName);
            aResponse.addHeader("Pargam", "no-cache");
            aResponse.addHeader("Cache-Control", "no-cache");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 通过页面导出
     *
     * @param aResponse
     * @throws IOException
     */
    public void export(HttpServletResponse aResponse) throws IOException {
        HSSFWorkbook tmpWorkbook = new HSSFWorkbook();
        String[] tmpUserTitles = {"姓名", "性别", "年龄", "工作"};
        aResponse.setHeader("content-type", "text/xls;charset=UTF-8");
        aResponse.setHeader("Content-disposition", String.format("attachment; filename=\"%s\"", "用户信息表_" + System.currentTimeMillis() + ".xls"));
        aResponse.setHeader("Pragma", "No-cache");//设置响应头信息，告诉浏览器不要缓存此内容
        aResponse.setHeader("Cache-Control", "no-cache");
        aResponse.setDateHeader("Expire", 0);
        List<String[]> tmpUsers = getUsers();
        createSheetData(tmpWorkbook, tmpUserTitles, "用户信息", tmpUsers);
//        setResponseHeader(aResponse, "用户信息表.xls");
        OutputStream tmpOutputStream = aResponse.getOutputStream();
        tmpWorkbook.write(tmpOutputStream);
        tmpOutputStream.flush();
        tmpOutputStream.close();
    }

    /**
     * 导出，不通过页面导出
     */
    public void export() throws IOException {
        HSSFWorkbook tmpWorkbook = new HSSFWorkbook();
        String[] tmpUserTitles = {"姓名", "性别", "年龄", "工作"};
        List<String[]> tmpUsers = getUsers();
        createSheetData(tmpWorkbook, tmpUserTitles, "用户信息", tmpUsers);
        String[] tmpAddressTitles = {"城市", "区域"};
        List<String[]> getAddress = getAddress();
        createSheetData(tmpWorkbook, tmpAddressTitles, "地址信息", getAddress);
        OutputStream tmpOutputStream = new FileOutputStream("C:\\" + System.currentTimeMillis() + ".xls");
        tmpWorkbook.write(tmpOutputStream);
        tmpOutputStream.flush();
        tmpOutputStream.close();
    }

    /**
     * 测试数据
     */
    private List<String[]> getAddress() {
        List<String[]> address = new ArrayList<>();
        for (int i = 1; i <= 5; i++) {
            String[] addr = new String[2];
            addr[0] = "四川";
            addr[1] = "高新 - " + i;
            address.add(addr);
        }
        return address;
    }

    /**
     * 测试数据
     */
    private List<String[]> getUsers() {
        List<String[]> users = new ArrayList<>();
        for (int i = 0; i < 10; i++) {
            String[] user = new String[4];
            user[0] = "zhangsan - " + i;
            user[1] = "男";
            user[2] = "2" + i;
            user[3] = "Java - " + i;
            users.add(user);
        }
        return users;
    }

    public void createExcel(HttpServletResponse response, List<TMonitorReport> list) {
        try {
            List<String> title = new ArrayList<>();
            title.add("ent_cert_type");
            title.add("bank_uid");
            title.add("bank_social_no");
            title.add("bank_ent_name");
            // 创建Excel的工作书册 Workbook,对应到一个excel文档
            HSSFWorkbook workbook = new HSSFWorkbook();
            HSSFCellStyle style = workbook.createCellStyle();
            // 生成一个字体
            HSSFFont font = workbook.createFont();
            // 字体增粗
            //font.setBoldweight(HSSFFont.BOLDWEIGHT_BOLD);
            font.setFontHeightInPoints((short) 11);
            style.setFont(font);
            // 创建Excel的工作sheet,对应到一个excel文档的tab
            HSSFSheet sheet = workbook.createSheet("sheet1");
            // 创建Excel的sheet的一行 (表头)
            HSSFRow row = sheet.createRow(0);
            // 表头内容填充
            for (int i = 0; i < title.size(); i++) {
                // 设置excel每列宽度
                sheet.setColumnWidth(i, 5000);
                HSSFCell cell = row.createCell(i);
                cell.setCellValue(title.get(i));
                cell.setCellStyle(style);
            }
            // 创建内容行
            HSSFCellStyle cellStyle = workbook.createCellStyle();
            cellStyle.setWrapText(true);// 自动换行
//            cellStyle.setDataFormat(HSSFDataFormat.getBuiltinFormat("0.00"));
            for (int j = 0; j < list.size(); j++) {
                HSSFRow contentRow = sheet.createRow(j + 1);
                TMonitorReport tMonitorReport = list.get(j);
                String entCertType = getCertType(tMonitorReport.getEntCertType());
                String bankUid = tMonitorReport.getBankUid();
                String bankSocialNo = tMonitorReport.getEntNo();
                String bankEntName = tMonitorReport.getEntName();
                for (int k = 0; k < title.size(); k++) {
                    HSSFCell cell = contentRow.createCell(k);
                    switch (k) {
                        case 0:
                            cell.setCellValue(entCertType);
                            break;
                        case 1:
                            cell.setCellValue(bankUid);
                            break;
                        case 2:
                            cell.setCellValue(bankSocialNo);
                            break;
                        case 3:
                            cell.setCellValue(bankEntName);
                            break;
                        default:
                            break;
                    }
                    cell.setCellStyle(cellStyle);
                }
            }
            writeExcelIo(response, workbook);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public void createExcelmportEntQz(HttpServletResponse response, List<ImportEntQz> list) {
        try {
            List<String> title = new ArrayList<>();
            title.add("bankUid");
            title.add("entName");
            title.add("NSRSBH");
            title.add("SHXYDM");
            // 创建Excel的工作书册 Workbook,对应到一个excel文档
            HSSFWorkbook workbook = new HSSFWorkbook();
            HSSFCellStyle style = workbook.createCellStyle();
            // 生成一个字体
            HSSFFont font = workbook.createFont();
            // 字体增粗
            //font.setBoldweight(HSSFFont.BOLDWEIGHT_BOLD);
            font.setFontHeightInPoints((short) 11);
            style.setFont(font);
            // 创建Excel的工作sheet,对应到一个excel文档的tab
            HSSFSheet sheet = workbook.createSheet("sheet1");
            // 创建Excel的sheet的一行 (表头)
            HSSFRow row = sheet.createRow(0);
            // 表头内容填充
            for (int i = 0; i < title.size(); i++) {
                // 设置excel每列宽度
                sheet.setColumnWidth(i, 5000);
                HSSFCell cell = row.createCell(i);
                cell.setCellValue(title.get(i));
                cell.setCellStyle(style);
            }
            // 创建内容行
            HSSFCellStyle cellStyle = workbook.createCellStyle();
            cellStyle.setWrapText(true);// 自动换行
//            cellStyle.setDataFormat(HSSFDataFormat.getBuiltinFormat("0.00"));
            for (int j = 0; j < list.size(); j++) {
                HSSFRow contentRow = sheet.createRow(j + 1);
                ImportEntQz importEntQz = list.get(j);
                String bankUid = importEntQz.getBankUid();
                String entName = importEntQz.getEntName();
                String NSRSBH = importEntQz.getNSRSBH();
                String SHXYDM = importEntQz.getSHXYDM();
                for (int k = 0; k < title.size(); k++) {
                    HSSFCell cell = contentRow.createCell(k);
                    switch (k) {
                        case 0:
                            cell.setCellValue(bankUid);
                            break;
                        case 1:
                            cell.setCellValue(entName);
                            break;
                        case 2:
                            cell.setCellValue(NSRSBH);
                            break;
                        case 3:
                            cell.setCellValue(SHXYDM);
                            break;
                        default:
                            break;
                    }
                    cell.setCellStyle(cellStyle);
                }
            }
            writeExcelIo(response, workbook);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /*泉州银行证件类型配置*/
    public String getCertType(String certType) {
        String entCertType = "";
        switch (certType) {
            case "0":
                entCertType = "身份证";
                break;
            case "1":
                entCertType = "户口簿";
                break;
            case "2":
                entCertType = "护照";
                break;
            case "3":
                entCertType = "军官证";
                break;
            case "4":
                entCertType = "士兵证";
                break;
            case "5":
                entCertType = "港澳居民来往内地通行证";
                break;
            case "6":
                entCertType = "台湾同胞来往内地通行证";
                break;
            case "7":
                entCertType = "临时身份证";
                break;
            case "8":
                entCertType = "外国人居留证";
                break;
            case "9":
                entCertType = "警官证";
                break;
            case "99":
                entCertType = "其他证件-个人";
                break;
            case "X":
                entCertType = "其他证件";
                break;
            case "a":
                entCertType = "组织机构代码证";
                break;
            case "b":
                entCertType = "营业执照";
                break;
            case "c":
                entCertType = "贷款卡";
                break;
            case "g":
                entCertType = "统一社会信用代码证";
                break;
            case "h":
                entCertType = "批文";
                break;
            case "i":
                entCertType = "主管证明";
                break;
            case "j":
                entCertType = "民办非企业登记证书";
                break;
            case "k":
                entCertType = "社会团体法人登记证书";
                break;
            case "l":
                entCertType = "事业单位法人登记证书";
                break;
            case "m":
                entCertType = "港澳台居民居住证";
                break;
            default:
                entCertType = "error";
                break;
        }
        return entCertType;
    }

    public void createImpQz(HttpServletResponse response, List<ImportEntQz> list) {
        try {
            List<String> title = new ArrayList<>();
            title.add("银行主键(bankUid)");
            title.add("企业ID(entUid)");
            title.add("企业名称(entName)");
            title.add("统一社会信用代码(entRegNo)");
            title.add("法定代表人(legalPerson)");

            title.add("登记机关(entRegUnit)");
            title.add("成立日期(entRegDt)");
            title.add("登记状态(entstatus)");
            title.add("企业类型(entType)");

            title.add("注册资本(entRegAmount)");
            title.add("单位(regcapunit)");
            title.add("币种(regcapcur)");
            title.add("营业期限(起止)(entdeadline)");

            title.add("核准日期(approvalDate)");
            title.add("企业地址(entAddress)");
            title.add("经营范围(entRange)");
            title.add("历史名称(entHisName)");

            title.add("所属行业(industryconame)");
            // 创建Excel的工作书册 Workbook,对应到一个excel文档
            HSSFWorkbook workbook = new HSSFWorkbook();
            HSSFCellStyle style = workbook.createCellStyle();
            //设置背景颜色
//            style.setFillForegroundColor(HSSFColor.LIME.index);
            style.setFillForegroundColor(HSSFColor.HSSFColorPredefined.GREEN.getIndex());
            style.setFillBackgroundColor(HSSFColor.HSSFColorPredefined.GREEN.getIndex());
            // 生成一个字体
            HSSFFont font = workbook.createFont();
            // 字体增粗
            //font.setBoldweight(HSSFFont.BOLDWEIGHT_BOLD);
            font.setFontHeightInPoints((short) 11);
            style.setFont(font);
            // 创建Excel的工作sheet,对应到一个excel文档的tab
            HSSFSheet sheet = workbook.createSheet("sheet1");
            // 创建Excel的sheet的一行 (表头)
            HSSFRow row = sheet.createRow(0);
            // 表头内容填充
            for (int i = 0; i < title.size(); i++) {
                // 设置excel每列宽度
                sheet.setColumnWidth(i, 5000);
                HSSFCell cell = row.createCell(i);
                cell.setCellValue(title.get(i));
                cell.setCellStyle(style);
            }
            // 创建内容行
            HSSFCellStyle cellStyle = workbook.createCellStyle();
            cellStyle.setWrapText(true);// 自动换行
//            cellStyle.setDataFormat(HSSFDataFormat.getBuiltinFormat("0.00"));
            for (int j = 0; j < list.size(); j++) {
                HSSFRow contentRow = sheet.createRow(j + 1);
                ImportEntQz importEntQz = list.get(j);
                String bankUid = importEntQz.getBankUid();
                String entName = importEntQz.getEntName();
                String NSRSBH = importEntQz.getNSRSBH();
                String SHXYDM = importEntQz.getSHXYDM();
                for (int k = 0; k < title.size(); k++) {
                    HSSFCell cell = contentRow.createCell(k);
                    switch (k) {
                        case 0:
                            cell.setCellValue(bankUid);
                            break;
                        case 2:
                            cell.setCellValue(entName.replaceAll("（", "(").replaceAll("）", ")").replaceAll("\\(车购税\\)", ""));
                            break;
                        case 3:
                            cell.setCellValue(SHXYDM);
                            break;
                        default:
                            break;
                    }
                    cell.setCellStyle(cellStyle);
                }
            }
            writeExcelIo(response, workbook);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /*通用输出方法*/
    public void writeExcelIo(HttpServletResponse response, HSSFWorkbook workbook) throws Exception {
        ByteArrayOutputStream os = new ByteArrayOutputStream();
        try {
            workbook.write(os);
        } catch (IOException e) {
            e.printStackTrace();
        }
        byte[] content = os.toByteArray();
        InputStream is = new ByteArrayInputStream(content);
        // 设置response参数，可以打开下载页面
//            response.setHeader("Content-Disposition" ,"attachment;filename="+new String(("tre"+".xls").getBytes(),"ISO-8859-1"));
//            response.setContentType("application/msexcel;charset=GBK");
        response.setContentType("application/msdownload");
        response.setCharacterEncoding("utf-8");
        response.setHeader("Content-disposition", "attachment; filename="
                + URLEncoder.encode("qz_report.xls", "UTF-8"));
        response.reset();
//            response.setContentType("application/vnd.ms-excel;charset=utf-8");
//            response.setHeader("Content-Disposition", "attachment;filename=" + new String(("qz_report" + ".xls").getBytes("gb2312"), "iso-8859-1"));
        ServletOutputStream out = response.getOutputStream();
        BufferedInputStream bis = null;
        BufferedOutputStream bos = null;
        try {
            bis = new BufferedInputStream(is);
            bos = new BufferedOutputStream(out);
            byte[] buff = new byte[2048];
            int bytesRead;
            // Simple read/write loop.
            while (-1 != (bytesRead = bis.read(buff, 0, buff.length))) {
                bos.write(buff, 0, bytesRead);
            }
        } catch (final IOException e) {
            throw e;
        } finally {
            if (bis != null) {
                bis.close();
            }
            if (bos != null) {
                bos.close();
            }
        }
    }

    public void exportBillingStatistics(HttpServletResponse response, List<BillingStatisticsMap> list) {
        try {
            List<String> title = new ArrayList<>();
            title.add("客户名称");
            title.add("接口名称");
            title.add("接口调用次数");
            title.add("接口单价");
            title.add("接口总价");
            title.add("统计周期");
            // 创建Excel的工作书册 Workbook,对应到一个excel文档
            HSSFWorkbook workbook = new HSSFWorkbook();
            HSSFCellStyle style = workbook.createCellStyle();
            // 生成一个字体
            HSSFFont font = workbook.createFont();
            // 字体增粗
            //font.setBoldweight(HSSFFont.BOLDWEIGHT_BOLD);
            font.setFontHeightInPoints((short) 11);
            style.setFont(font);
            // 创建Excel的工作sheet,对应到一个excel文档的tab
            HSSFSheet sheet = workbook.createSheet("sheet1");
            // 创建Excel的sheet的一行 (表头)
            HSSFRow row = sheet.createRow(0);
            // 表头内容填充
            for (int i = 0; i < title.size(); i++) {
                // 设置excel每列宽度
                sheet.setColumnWidth(i, 5000);
                HSSFCell cell = row.createCell(i);
                cell.setCellValue(title.get(i));
                cell.setCellStyle(style);
            }
            // 创建内容行
            HSSFCellStyle cellStyle = workbook.createCellStyle();
            cellStyle.setWrapText(true);// 自动换行
//            cellStyle.setDataFormat(HSSFDataFormat.getBuiltinFormat("0.00"));
            for (int j = 0; j < list.size(); j++) {
                HSSFRow contentRow = sheet.createRow(j + 1);
                BillingStatisticsMap billingStatisticsMap = list.get(j);
                for (int k = 0; k < title.size(); k++) {
                    HSSFCell cell = contentRow.createCell(k);
                    switch (k) {
                        case 0:
                            cell.setCellValue(billingStatisticsMap.getUserName());
                            break;
                        case 1:
                            cell.setCellValue(billingStatisticsMap.getDataitemUName());
                            break;
                        case 2:
                            cell.setCellValue(billingStatisticsMap.getCount());
                            break;
                        case 3:
                            cell.setCellValue(billingStatisticsMap.getActualPrice() + "");
                            break;
                        case 4:
                            cell.setCellValue(billingStatisticsMap.getAllPrice() + "");
                            break;
                        case 5:
                            cell.setCellValue(billingStatisticsMap.getRangeTime());
                            break;
                        default:
                            break;
                    }
                    cell.setCellStyle(cellStyle);
                }
            }
            writeExcelIo(response, workbook);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public void exportBillingStatisticsDetail(HttpServletResponse response, List<BillingStatisticsDetailMap> list) {
        try {
            List<String> title = new ArrayList<>();
            title.add("用户名称");
            title.add("接口名称");
            title.add("企业名称");
            title.add("调用日期");
            title.add("有无内容");
            title.add("返回结果");
            title.add("接口单价（元）");
            // 创建Excel的工作书册 Workbook,对应到一个excel文档
            HSSFWorkbook workbook = new HSSFWorkbook();
            HSSFCellStyle style = workbook.createCellStyle();
            // 生成一个字体
            HSSFFont font = workbook.createFont();
            // 字体增粗
            //font.setBoldweight(HSSFFont.BOLDWEIGHT_BOLD);
            font.setFontHeightInPoints((short) 11);
            style.setFont(font);
            // 创建Excel的工作sheet,对应到一个excel文档的tab
            HSSFSheet sheet = workbook.createSheet("sheet1");
            // 创建Excel的sheet的一行 (表头)
            HSSFRow row = sheet.createRow(0);
            // 表头内容填充
            for (int i = 0; i < title.size(); i++) {
                // 设置excel每列宽度
                sheet.setColumnWidth(i, 5000);
                HSSFCell cell = row.createCell(i);
                cell.setCellValue(title.get(i));
                cell.setCellStyle(style);
            }
            // 创建内容行
            HSSFCellStyle cellStyle = workbook.createCellStyle();
            cellStyle.setWrapText(true);// 自动换行
//            cellStyle.setDataFormat(HSSFDataFormat.getBuiltinFormat("0.00"));
            for (int j = 0; j < list.size(); j++) {
                HSSFRow contentRow = sheet.createRow(j + 1);
                BillingStatisticsDetailMap billingStatisticsDetailMap = list.get(j);
                for (int k = 0; k < title.size(); k++) {
                    HSSFCell cell = contentRow.createCell(k);
                    switch (k) {
                        case 0:
                            cell.setCellValue(billingStatisticsDetailMap.getUserName());
                            break;
                        case 1:
                            cell.setCellValue(billingStatisticsDetailMap.getDataitemUName());
                            break;
                        case 2:
                            cell.setCellValue(billingStatisticsDetailMap.getMonitorEntName());
                            break;
                        case 3:
                            cell.setCellValue(billingStatisticsDetailMap.getSendTime());
                            break;
                        case 4:
                            if (billingStatisticsDetailMap.getHasContent() == 1) {
                                cell.setCellValue("有");
                            } else {
                                cell.setCellValue("无");
                            }
                            break;
                        case 5:
                            if (billingStatisticsDetailMap.getSendStatus() == 1) {
                                cell.setCellValue("正常");
                            } else {
                                cell.setCellValue("异常");
                            }
                            break;
                        case 6:
                            cell.setCellValue(billingStatisticsDetailMap.getActualPrice() + "");
                            break;
                        default:
                            break;
                    }
                    cell.setCellStyle(cellStyle);
                }
            }
            writeExcelIo(response, workbook);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public void exportTotalStatisticsDetail(HttpServletResponse response, List<TotalStatisticsMap> list) {
        try {
            List<String> title = new ArrayList<>();
            title.add("企业ID");
            title.add("企业名称");
            title.add("用户名");
            title.add("用户ID");
            title.add("数据项名");
            title.add("数据编码");
            title.add("接口单价（元）");
            title.add("数量");
            title.add("总价（元）");
            // 创建Excel的工作书册 Workbook,对应到一个excel文档
            HSSFWorkbook workbook = new HSSFWorkbook();
            HSSFCellStyle style = workbook.createCellStyle();
            // 生成一个字体
            HSSFFont font = workbook.createFont();
            // 字体增粗
            //font.setBoldweight(HSSFFont.BOLDWEIGHT_BOLD);
            font.setFontHeightInPoints((short) 11);
            style.setFont(font);
            // 创建Excel的工作sheet,对应到一个excel文档的tab
            HSSFSheet sheet = workbook.createSheet("sheet1");
            // 创建Excel的sheet的一行 (表头)
            HSSFRow row = sheet.createRow(0);
            // 表头内容填充
            for (int i = 0; i < title.size(); i++) {
                // 设置excel每列宽度
                sheet.setColumnWidth(i, 5000);
                HSSFCell cell = row.createCell(i);
                cell.setCellValue(title.get(i));
                cell.setCellStyle(style);
            }
            // 创建内容行
            HSSFCellStyle cellStyle = workbook.createCellStyle();
            cellStyle.setWrapText(true);// 自动换行
//            cellStyle.setDataFormat(HSSFDataFormat.getBuiltinFormat("0.00"));
            for (int j = 0; j < list.size(); j++) {
                HSSFRow contentRow = sheet.createRow(j + 1);
                TotalStatisticsMap totalStatisticsDetailMap = list.get(j);
                for (int k = 0; k < title.size(); k++) {
                    HSSFCell cell = contentRow.createCell(k);
                    switch (k) {
                        case 0:
                            cell.setCellValue(totalStatisticsDetailMap.getEntUid());
                            break;
                        case 1:
                            cell.setCellValue(totalStatisticsDetailMap.getMonitorEntName());
                            break;
                        case 2:
                            cell.setCellValue(totalStatisticsDetailMap.getUserName());
                            break;
                        case 3:
                            cell.setCellValue(totalStatisticsDetailMap.getUserId());
                            break;
                        case 4:
                            cell.setCellValue(totalStatisticsDetailMap.getDataitemUName());
                            break;
                        case 5:
                            cell.setCellValue(totalStatisticsDetailMap.getDynamicDataCode());
                            break;
                        case 6:
                            cell.setCellValue(totalStatisticsDetailMap.getActualPrice() + "");
                            break;
                        case 7:
                            cell.setCellValue(totalStatisticsDetailMap.getCount());
                            break;
                        case 8:
                            cell.setCellValue(totalStatisticsDetailMap.getTotalPrice() + "");
                            break;
                        default:
                            break;
                    }
                    cell.setCellStyle(cellStyle);
                }
            }
            writeExcelIo(response, workbook);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public void excelTStatisticsEnt(HttpServletResponse response, List<TStatisticsEnt> list) {
        try {
            List<String> title = new ArrayList<>();
            title.add("指标大类");
            title.add("指标小类");
            title.add("开始时间");
            title.add("结束时间");
            title.add("数量");
            // 创建Excel的工作书册 Workbook,对应到一个excel文档
            HSSFWorkbook workbook = new HSSFWorkbook();
            HSSFCellStyle style = workbook.createCellStyle();
            // 生成一个字体
            HSSFFont font = workbook.createFont();
            // 字体增粗
            //font.setBoldweight(HSSFFont.BOLDWEIGHT_BOLD);
            font.setFontHeightInPoints((short) 8);
            style.setFont(font);
            // 创建Excel的工作sheet,对应到一个excel文档的tab
            HSSFSheet sheet = workbook.createSheet("sheet1");
            // 创建Excel的sheet的一行 (表头)
            HSSFRow row = sheet.createRow(0);
            // 表头内容填充
            HSSFCellStyle styleBt = workbook.createCellStyle();
            HSSFFont fontBt = workbook.createFont();
            // 字体增粗
            fontBt.setBold(true);
            fontBt.setFontHeightInPoints((short) 10);
            styleBt.setFont(fontBt);
            styleBt.setFillForegroundColor(IndexedColors.SEA_GREEN.getIndex());//背景
            styleBt.setFillPattern(FillPatternType.SOLID_FOREGROUND);
//            styleBt .setAlignment(HorizontalAlignment.CENTER);
//            styleBt .setDataFormat(HSSFDataFormat.getBuiltinFormat("0.00%"));
//            styleBt.setFillBackgroundColor((short) 59);
//            styleBt.setFillForegroundColor((short) 59);
            for (int i = 0; i < title.size(); i++) {
                // 设置excel每列宽度
//                sheet.setColumnWidth(i, 5000);
                sheet.autoSizeColumn(i);
                if (i == 1) {
                    sheet.setColumnWidth(i, 10000);
                }
                if (i == 2 || i == 3) {
                    sheet.setColumnWidth(i, 5000);
                }
                HSSFCell cell = row.createCell(i);
                cell.setCellValue(title.get(i));
                cell.setCellStyle(styleBt);
            }
            // 创建内容行
            HSSFCellStyle cellStyle = workbook.createCellStyle();
            cellStyle.setWrapText(true);// 自动换行
//            cellStyle.setDataFormat(HSSFDataFormat.getBuiltinFormat("0.00"));
            SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");
            for (int j = 0; j < list.size(); j++) {
                HSSFRow contentRow = sheet.createRow(j + 1);
                TStatisticsEnt tStatisticsEnt = list.get(j);
                for (int k = 0; k < title.size(); k++) {
                    HSSFCell cell = contentRow.createCell(k);
                    switch (k) {
                        case 0:
                            cell.setCellValue(tStatisticsEnt.getSupCodeName());
                            break;
                        case 1:
                            cell.setCellValue(tStatisticsEnt.getSubCodeName());
                            break;
                        case 2:
                            if (tStatisticsEnt.getStartTime() != null) {
                                cell.setCellValue(simpleDateFormat.format(tStatisticsEnt.getStartTime()));
                            } else {
                                cell.setCellValue("-");
                            }
                            break;
                        case 3:
                            if (tStatisticsEnt.getEndTime() != null) {
                                cell.setCellValue(simpleDateFormat.format(tStatisticsEnt.getEndTime()));
                            } else {
                                cell.setCellValue("-");
                            }
                            break;
                        case 4:
                            cell.setCellValue(tStatisticsEnt.getCount());
                            break;
                        default:
                            break;
                    }
                    cell.setCellStyle(cellStyle);
                }
            }
            writeExcelIo(response, workbook);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public void exportPartnersForNSH(HttpServletResponse response, List<PartnersRequestLog> list) {
        try {
            List<String> title = new ArrayList<>();
            title.add("客户名称");
            title.add("查询日期");
            title.add("查询内容");
            title.add("返回内容");
            title.add("接口状态");
            // 创建Excel的工作书册 Workbook,对应到一个excel文档
            HSSFWorkbook workbook = new HSSFWorkbook();
            HSSFCellStyle style = workbook.createCellStyle();
            // 生成一个字体
            HSSFFont font = workbook.createFont();
            // 字体增粗
            //font.setBoldweight(HSSFFont.BOLDWEIGHT_BOLD);
            font.setFontHeightInPoints((short) 8);
            style.setFont(font);
            // 创建Excel的工作sheet,对应到一个excel文档的tab
            HSSFSheet sheet = workbook.createSheet("sheet1");
            // 创建Excel的sheet的一行 (表头)
            HSSFRow row = sheet.createRow(0);
            // 表头内容填充
            HSSFCellStyle styleBt = workbook.createCellStyle();
            HSSFFont fontBt = workbook.createFont();
            // 字体增粗
            fontBt.setBold(true);
            fontBt.setFontHeightInPoints((short) 10);
            styleBt.setFont(fontBt);
            styleBt.setFillForegroundColor(IndexedColors.SEA_GREEN.getIndex());//背景
            styleBt.setFillPattern(FillPatternType.SOLID_FOREGROUND);
            for (int i = 0; i < title.size(); i++) {
                // 设置excel每列宽度
                sheet.setColumnWidth(i, 5000);
                sheet.autoSizeColumn(i);
//                if(i==1){
//                    sheet.setColumnWidth(i, 10000);
//                }
//                if(i==2||i==3){
//                    sheet.setColumnWidth(i, 5000);
//                }
                HSSFCell cell = row.createCell(i);
                cell.setCellValue(title.get(i));
                cell.setCellStyle(styleBt);
            }
            // 创建内容行
            HSSFCellStyle cellStyle = workbook.createCellStyle();
            cellStyle.setWrapText(true);// 自动换行
//            cellStyle.setDataFormat(HSSFDataFormat.getBuiltinFormat("0.00"));
            SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");
            for (int j = 0; j < list.size(); j++) {
                HSSFRow contentRow = sheet.createRow(j + 1);
                PartnersRequestLog partnersRequestLog = list.get(j);
                for (int k = 0; k < title.size(); k++) {
                    HSSFCell cell = contentRow.createCell(k);
                    switch (k) {
                        case 0:
                            cell.setCellValue(partnersRequestLog.getPartner());
                            break;
                        case 1:
                            cell.setCellValue(partnersRequestLog.getDate());
                            break;
                        case 2:
                            String request = partnersRequestLog.getRequest();
                            if (request != null) {
                                try {
                                    JSONObject jsonObject = JSONObject.parseObject(request);
                                    cell.setCellValue(jsonObject.get("entName") + "");
                                } catch (Exception e) {
                                    cell.setCellValue("-");
                                }
                            } else {
                                cell.setCellValue("-");
                            }
                            break;
                        case 3:
                            if (partnersRequestLog.getResponse() != null) {
                                cell.setCellValue(partnersRequestLog.getResponse());
                            } else {
                                cell.setCellValue("-");
                            }
                            break;
                        case 4:
                            String status = partnersRequestLog.getStatus();
                            if (StringUtils.isNotEmpty(status)) {
                                if ("0".equals(status)) {
                                    cell.setCellValue("成功");
                                } else {
                                    cell.setCellValue("异常");
                                }
                            } else {
                                cell.setCellValue("异常");
                            }
                            break;
                        default:
                            break;
                    }
                    cell.setCellStyle(cellStyle);
                }
            }
            writeExcelIo(response, workbook);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public void excelBatchNoticeFileData(HttpServletResponse response, List<BatchNoticeFileData> list) {
        try {
            List<String> title = new ArrayList<>();
            title.add("序号");
            title.add("企业名称");
            title.add("统一社会信用代码");
            title.add("数据主键");
            // 创建Excel的工作书册 Workbook,对应到一个excel文档
            HSSFWorkbook workbook = new HSSFWorkbook();
            HSSFCellStyle style = workbook.createCellStyle();
            // 生成一个字体
            HSSFFont font = workbook.createFont();
            // 字体增粗
            //font.setBoldweight(HSSFFont.BOLDWEIGHT_BOLD);
            font.setFontHeightInPoints((short) 15);
            style.setFont(font);
            // 创建Excel的工作sheet,对应到一个excel文档的tab
            HSSFSheet sheet = workbook.createSheet("批量数据供给管理");
            // 创建Excel的sheet的一行 (表头)
            HSSFRow row = sheet.createRow(0);
            // 表头内容填充
            HSSFCellStyle styleBt = workbook.createCellStyle();
            HSSFFont fontBt = workbook.createFont();
            // 字体增粗
            fontBt.setBold(true);
            fontBt.setFontHeightInPoints((short) 18);
            styleBt.setFont(fontBt);
            styleBt.setFillForegroundColor(IndexedColors.SEA_GREEN.getIndex());//背景
            styleBt.setFillPattern(FillPatternType.SOLID_FOREGROUND);
//            styleBt .setAlignment(HorizontalAlignment.CENTER);
//            styleBt .setDataFormat(HSSFDataFormat.getBuiltinFormat("0.00%"));
//            styleBt.setFillBackgroundColor((short) 59);
//            styleBt.setFillForegroundColor((short) 59);
            for (int i = 0; i < title.size(); i++) {
                // 设置excel每列宽度
//                sheet.setColumnWidth(i, 5000);
                sheet.autoSizeColumn(i);
                if (i == 1) {
                    sheet.setColumnWidth(i, 15000);
                }
                if (i == 2) {
                    sheet.setColumnWidth(i, 10000);
                }
                if (i == 3) {
                    sheet.setColumnWidth(i, 5000);
                }
                HSSFCell cell = row.createCell(i);
                cell.setCellValue(title.get(i));
                cell.setCellStyle(styleBt);
            }
            // 创建内容行
            HSSFCellStyle cellStyle = workbook.createCellStyle();
            cellStyle.setWrapText(true);// 自动换行
//            cellStyle.setDataFormat(HSSFDataFormat.getBuiltinFormat("0.00"));
            SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");
            for (int j = 0; j < list.size(); j++) {
                HSSFRow contentRow = sheet.createRow(j + 1);
                BatchNoticeFileData noticeFileData = list.get(j);
                for (int k = 0; k < title.size(); k++) {
                    HSSFCell cell = contentRow.createCell(k);
                    switch (k) {
                        case 0:
                            cell.setCellValue(j + 1);
                            break;
                        case 1:
                            if (StringUtils.isNotBlank(noticeFileData.getEntName())) {
                                cell.setCellValue(noticeFileData.getEntName());
                            } else {
                                cell.setCellValue("-");
                            }
                            cell.setCellValue(noticeFileData.getEntName());
                            break;
                        case 2:
                            if (StringUtils.isNotBlank(noticeFileData.getEntSocialCode())) {
                                cell.setCellValue(noticeFileData.getEntSocialCode());
                            } else {
                                cell.setCellValue("-");
                            }
                            break;
                        case 3:
                            if (noticeFileData.getId() != null) {
                                cell.setCellValue(noticeFileData.getId());
                            } else {
                                cell.setCellValue("-");
                            }
                            break;
                        default:
                            break;
                    }
                    cell.setCellStyle(cellStyle);
                }
            }
            writeExcelIo(response, workbook);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    public void exportChargeReportExcel(HttpServletResponse response, List<ExportChargeReportVo> list, ExportChargeReportReq req) {
        try {
            Map<String, String> title = new HashMap<>();
            title.put("tab0", "搜索条");
            title.put("tab1", "客户名称");
            title.put("tab2", "产品编码");
            title.put("tab3", "产品名称");
            title.put("count0", "今年累计");
            title.put("count1", "所有累计");
            title.put("subtab3", "请求数量");
            title.put("subtab4", "计费请求数量");
            title.put("subtab5", "不计费请求数量");
            title.put("subtab6", "请求数量");
            title.put("subtab7", "计费请求数量");
            title.put("subtab8", "不计费请求数量");
            // 创建Excel的工作书册 Workbook,对应到一个excel文档
            HSSFWorkbook workbook = new HSSFWorkbook();
            HSSFCellStyle style = workbook.createCellStyle();
            // 生成一个字体
            HSSFFont font = workbook.createFont();
            // 字体增粗
            //font.setBoldweight(HSSFFont.BOLDWEIGHT_BOLD);
            font.setFontHeightInPoints((short) 13);
            style.setFont(font);
            // 创建Excel的工作sheet,对应到一个excel文档的tab
            HSSFSheet sheet = workbook.createSheet("接口调用情况统计");
            // 表头内容填充
            // 创建Excel的sheet的一行 (表头)
            HSSFRow row = sheet.createRow(0);
            // 创建Excel的sheet的第二行
            HSSFRow row1 = sheet.createRow(1);
            // 搜索条 客户名称 产品名称
            for (int i = 0; i < 5; i++) {
                HSSFCell cell = null;
                if (i == 0) {
                    cell = row.createCell(i);
                    cell.setCellValue(title.get("tab" + i));
                } else if (i == 2){
                    cell = row1.createCell(i);
                    cell.setCellValue(req.getUserName());
                } else if (i == 4) {
                    cell = row1.createCell(i);
                    cell.setCellValue(req.getProductName());
                } else {
                    cell = row1.createCell(i);
                    cell.setCellValue(title.get("tab" + i));
                }
                cell.setCellStyle(style);
            }
            HSSFCellStyle styleBt = workbook.createCellStyle();
            HSSFFont fontBt = workbook.createFont();
            // 字体增粗
            fontBt.setBold(true);
            fontBt.setFontHeightInPoints((short) 13);
            fontBt.setColor(IndexedColors.WHITE.getIndex());
            styleBt.setFont(fontBt);
            //设置样式对齐方式：水平垂直居中
            styleBt.setAlignment(HorizontalAlignment.CENTER);
            styleBt.setVerticalAlignment(VerticalAlignment.CENTER);
            //设定填充单色
            styleBt.setFillPattern(FillPatternType.SOLID_FOREGROUND);
            //设定背景颜色
            styleBt.setFillForegroundColor(IndexedColors.PALE_BLUE.getIndex());
//            this.setBorderColor(styleBt, IndexedColors.BLACK.getIndex(), BorderStyle.MEDIUM);
            //创建行，指定起始行号，从0开始
            HSSFRow row2 = sheet.createRow(2);
            // 客户名称 产品编码 产品名称 合并单元格
            for (int i = 0; i < 3; i++) {
                sheet.autoSizeColumn(i);
                sheet.setColumnWidth(i, 6000);
                //指定合并开始行、合并结束行 合并开始列、合并结束列
                CellRangeAddress rangeAddress = new CellRangeAddress(2, 3, i, i);
                //添加要合并地址到表格
                sheet.addMergedRegion(rangeAddress);
                //创建单元格，指定起始列号，从0开始
                HSSFCell cell = row2.createCell(i);
                //设置单元格内容
                cell.setCellValue(title.get("tab" + (i + 1)));
                cell.setCellStyle(styleBt);
            }
            // 今年累计  所有累计  合并单元格
            for (int i = 0; i < 2; i++) {
                //指定合并开始行、合并结束行 合并开始列、合并结束列
                CellRangeAddress rangeAddress = new CellRangeAddress(2, 2, i * 3 + 3, i * 3 + 5);
                //添加要合并地址到表格
                sheet.addMergedRegion(rangeAddress);
                //创建单元格，指定起始列号，从0开始
                HSSFCell cell = row2.createCell(i * 3 + 3);
                //设置单元格内容
                cell.setCellValue(title.get("count" + i));
                cell.setCellStyle(styleBt);
            }
            HSSFRow row3 = sheet.createRow(3);
            // 请求数量 计费请求数量 不计费请求数量
            for (int i = 3; i < 9; i++) {
                // 设置excel每列宽度
                sheet.autoSizeColumn(i);
                sheet.setColumnWidth(i, 6000);
                HSSFCell cell = row3.createCell(i);
                cell.setCellValue(title.get("subtab" + i));
                cell.setCellStyle(styleBt);
            }
            // 创建内容行
            HSSFCellStyle cellStyle = workbook.createCellStyle();
            cellStyle.setWrapText(true);// 自动换行
            font.setFontHeightInPoints((short) 11);
            cellStyle.setFont(font);
            for (int j = 0; j < list.size(); j++) {
                HSSFRow contentRow = sheet.createRow(j + 4);
                ExportChargeReportVo chargeReport = list.get(j);
                for (int k = 0; k < 9; k++) {
                    HSSFCell cell = contentRow.createCell(k);
                    switch (k) {
                        case 0:
                            if (StringUtils.isNotBlank(chargeReport.getReportAccountName())) {
                                cell.setCellValue(chargeReport.getReportAccountName());
                            } else {
                                cell.setCellValue("-");
                            }
                            break;
                        case 1:
                            if (StringUtils.isNotBlank(chargeReport.getReportDataCode())) {
                                cell.setCellValue(chargeReport.getReportDataCode());
                            } else {
                                cell.setCellValue("-");
                            }
                            break;
                        case 2:
                            if (StringUtils.isNotBlank(chargeReport.getReportDataName())) {
                                cell.setCellValue(chargeReport.getReportDataName());
                            } else {
                                cell.setCellValue("-");
                            }
                            break;
                        case 3:
                            if (chargeReport.getReportNum() != null) {
                                cell.setCellValue(chargeReport.getReportNum());
                            } else {
                                cell.setCellValue("-");
                            }
                            break;
                        case 4:
                            if (chargeReport.getReportFeeNum() != null) {
                                cell.setCellValue(chargeReport.getReportFeeNum());
                            } else {
                                cell.setCellValue("-");
                            }
                            break;
                        case 5:
                            if (chargeReport.getReportFreeNum() != null) {
                                cell.setCellValue(chargeReport.getReportFreeNum());
                            } else {
                                cell.setCellValue("-");
                            }
                            break;
                        case 6:
                            if (chargeReport.getReportAllNum() != null) {
                                cell.setCellValue(chargeReport.getReportAllNum());
                            } else {
                                cell.setCellValue("-");
                            }
                            break;
                        case 7:
                            if (chargeReport.getReportFeeAllNum() != null) {
                                cell.setCellValue(chargeReport.getReportFeeAllNum());
                            } else {
                                cell.setCellValue("-");
                            }
                            break;
                        case 8:
                            if (chargeReport.getReportFreeAllNum() != null) {
                                cell.setCellValue(chargeReport.getReportFreeAllNum());
                            } else {
                                cell.setCellValue("-");
                            }
                            break;
                        default:
                            break;
                    }
                    cell.setCellStyle(cellStyle);
                }
            }
            writeExcelIo(response, workbook);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

}

```

### ExcelExportUtil对应controller类

```java
  @ApiOperation("接口调用情况统计-导出数据")
    @PostMapping("exportV2")
    public void exportV2(@RequestBody ExportChargeReportReq req,
                       HttpServletRequest request, HttpServletResponse response) throws IOException {
        log.info("对外API 接口调用情况统计-导出数据,请求参数:{}", JSONObject.toJSONString(req));
        List<ExportChargeReportVo> exportChargeReport = chargeReportService.getExportChargeReport(req.getUserName(), req.getProductName());
        log.info("查得结果条数：{}\n 查询结果：{}", exportChargeReport.size(), exportChargeReport);
        if (!CollectionUtils.isEmpty(exportChargeReport)) {
            //将选中数据导出到excel
            ExcelExportUtil excelExportUtil = new ExcelExportUtil();
            excelExportUtil.exportChargeReportExcel(response, exportChargeReport, req);
        }
    }
```

#### Excel图片



![](/images/pasted-3.png)



### 传参数导出

```java
    @ApiOperation("导出")
    @PostMapping("export")
    public void export(@RequestBody WinProjectReq winProjectReq, HttpServletResponse response) throws IOException {
//查询要导出的数据
        List<WinnerExportVo> list = winProjectService.selectExportList(winProjectReq);
        ExcelUtil<WinnerExportVo> util = new ExcelUtil<WinnerExportVo>(WinnerExportVo.class);
        util.exportExcel(response,list, "中标企业名单导出");
    }
```



## TK-Mybaties常用方法


![](/images/pasted-4.png)

### 1.查全部

```java 
//查全部
public List<SysUser> getUserList() {
    return sysUserMapper.selectAll();
}
```

### 2.根据主键id查询

```java
//根据主键id查询
public SysUser getUserById(Long id) {
    return  sysUserMapper.selectByPrimaryKey(id);
}
```

### 3.根据主键id选择更新

```java
//根据主键id选择更新    
public void update(SysUser user) {
    sysUserMapper.updateByPrimaryKeySelective(user);
}
```

### 4.根据主键id更新所有

```java
//根据主键id更新所有
public void update(SysUser user) {
    sysUserMapper.updateByPrimaryKey(user);
}
```

### 5.根据主键id删除

```java
//根据主键id删除
public void delete(Long id) {
    sysUserMapper.deleteByPrimaryKey(id);
}
```

### 6.根据条件精确查询

```java
//根据条件精确查询    
public List<SysUser> getUserList(String loginName) {
    Example example = new Example(SysUser.class);
    if(StringUtils.isNotBlank(loginName)){
        example.createCriteria().andEqualTo("loginName", loginName);
    }
    return sysUserMapper.selectByExample(example);
}
```

### 7.根据条件模糊查询

```java
//根据条件模糊查询    
public List<SysUser> getUserList(String loginName) {
    Example example = new Example(SysUser.class);
    if(StringUtils.isNotBlank(loginName)){
        example.createCriteria().andLike("loginName", "%" + loginName + "%");
    }
    return sysUserMapper.selectByExample(example);
}
```

### 8.根据时间查询并排序

```java
//根据时间查询并排序    
public List<SysUser> getUserList(String startDate, String endDate) {
    Example example = new Example(SysUser.class);
    Example.Criteria criteria = example.createCriteria();
    if (StringUtils.isNotBlank(startDate)) {
        criteria.andGreaterThanOrEqualTo("createTime", startDate);
    }
    if (StringUtils.isNotBlank(endDate)) {
        criteria.andLessThanOrEqualTo("createTime", endDate);
    }
    example.setOrderByClause("create_time desc");
    return sysUserMapper.selectByExample(example);
}
```

### 9.根据条件选择更新

```java
//根据条件选择更新    
public void update(SysUser user) {
    Example example = new Example(SysUser.class);
    example.createCriteria().andEqualTo("loginName", user.getLoginName());
    sysUserMapper.updateByExampleSelective(user, example);
}
```

### 10.根据条件更新所有

```java
//根据条件更新所有    
public void update(SysUser user) {
    Example example = new Example(SysUser.class);
    example.createCriteria().andEqualTo("loginName", user.getLoginName());
    sysUserMapper.updateByExample(user, example);
}
```

### 11.根据条件删除

```java
//根据条件删除    
public void delete(String loginName) {
    Example example = new Example(SysUser.class);
    example.createCriteria().andEqualTo("loginName", loginName);
    sysUserMapper.deleteByExample(example);
}
```

### 12.复杂条件查询

```java
//复杂条件查询    
public List<SysUser> getUserList(String loginName, String password) {
    Example example = new Example(SysUser.class);
    Example.Criteria criteria1 = example.createCriteria();
    criteria1.andEqualTo("loginName", loginName);
    criteria1.andEqualTo("password", password);
    Example.Criteria criteria2 = example.createCriteria();
    criteria2.andEqualTo("email", loginName);
    criteria2.andEqualTo("password", password);
    example.or(criteria2);
    return sysUserMapper.selectByExample(example);
}
```

### 13.多表查询

在mapper文件定义方法名和SQL语句，使用方法:注入对应mapper后，用xxxmapper.方法名。

```java
import com.psds.credit.entity.WarningRules;
import com.psds.credit.model.FindNamesBody;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Select;
import tk.mybatis.mapper.common.Mapper;

import java.util.List;

public interface WarningRulesMapper extends Mapper<WarningRules> {

    @Select("SELECT YJGZ_Name rulesName FROM T_XYYJ_YJGZ WHERE YJGZ_YJLX_Code = '${statisticType}' AND YJGZ_YJWD_Code = '${statisticLatitude}'")
    List<String> selectRulesName(@Param("statisticType") String statisticType, @Param("statisticLatitude") String statisticLatitude);

    /**
     *  预警统计任务，查询规则名称、预警类型名称、预警纬度名称
     */
    @Select("select a.YJGZ_Name as 'rulesName',b.YJLX_MC as 'warningTypeName',c.YJWD_MC as 'latitudeName' from T_XYYJ_YJGZ a INNER JOIN T_XYYJ_YJLX b on a.YJGZ_YJLX_Code=b.YJLX_BM INNER JOIN T_XYYJ_YJWD c on a.YJGZ_YJWD_Code=c.YJWD_BM where a.YJGZ_ID='${recordRulesId}'")
    List<FindNamesBody> findNames(@Param("recordRulesId") int recordRulesId);
}
```



## 定时任务

本地调试参考：https://www.cnblogs.com/mmzs/p/10161936.html

```java
package com.psds.credit.quartz;
import com.baomidou.dynamic.datasource.annotation.DS;
import com.psds.credit.common.core.controller.BaseController;
import com.psds.credit.common.utils.StringUtils;
import com.psds.credit.entity.WarningRecord;
import com.psds.credit.entity.WarningStatistics;
import com.psds.credit.mapper.WarningRecordMapper;
import com.psds.credit.mapper.WarningRulesMapper;
import com.psds.credit.mapper.WarningStatisticsMapper;
import com.psds.credit.model.FindNamesBody;
import com.psds.credit.service.WarningStatisticsService;
import org.apache.commons.lang3.time.DateFormatUtils;
import org.apache.commons.lang3.time.DateUtils;
import org.springframework.stereotype.Component;
import tk.mybatis.mapper.entity.Example;
import javax.annotation.Resource;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Date;
import java.util.List;
import java.util.stream.Collectors;

/**
 * 预警统计定时任务调度
 *
 * @author ChenZhiMing
 */
@Component("ryWarningStatistics")
@DS("XYYJ")
public class RyWarningStatistics extends BaseController {
    @Resource
    private WarningRecordMapper warningRecordMapper;
    @Resource
    private WarningStatisticsService warningStatisticsService;
    @Resource
    private WarningRulesMapper warningRulesMapper;
    @Resource
    private WarningStatisticsMapper warningStatisticsMapper;

    /**
     * 根据月或日统计
     *
     * @param type:
     * @return
     */
    public void warningStatistics(String type) {
        Date date = new Date();
        Example example = new Example(WarningRecord.class);
        Example.Criteria criteria = example.createCriteria();
        WarningStatistics warningStatistics = new WarningStatistics();
        criteria.andGreaterThanOrEqualTo("createTime", DateFormatUtils.format(date, "yyyy-MM-dd"));
        criteria.andLessThan("createTime", DateFormatUtils.format(DateUtils.addDays(date, 1), "yyyy-MM-dd"));
        List<WarningRecord> warningRecordsList = warningRecordMapper.selectByExample(example);
        if (warningRecordsList.size() > 0) {
            if ("日".equals(type)) {
                if (warningRecordsList.size() > 0) {
                    warningStatistics.setUserId(warningRecordsList.get(0).getRecordPersonId());
                    warningStatistics.setWarningStatisticsTime(date);
                    warningStatistics.setWarningStatisticsType("日");
                    warningStatistics.setWarningStatisticsName("统计名称" + DateFormatUtils.format(date, "yyyyMMddHHmmss"));
                    //根据规则id查预警纬度名称、预警规则名称、预警类型名称
                    List<FindNamesBody> names = new ArrayList<>();
                    for (WarningRecord warningRecord : warningRecordsList) {
                        names.addAll(warningRulesMapper.findNames(warningRecord.getRecordRulesId()));
                    }
                    //转换、去重
                    List<String> warningResultName = names.stream().distinct().map(resultName -> resultName.getRulesName()).collect(Collectors.toList());
                    List<String> warningTypeList = names.stream().map(typeName -> typeName.getWarningTypeName()).distinct().collect(Collectors.toList());
                    List<String> warningLatitudeList = names.stream().map(latitudeName -> latitudeName.getLatitudeName()).distinct().collect(Collectors.toList());
                    warningStatistics.setWarningStatisticsLatitude(warningResultName.toString() + warningTypeList + warningLatitudeList);
                    warningStatistics.setWarningStatisticsCount(warningRecordsList.size());
                    warningStatistics.setCreateTime(date);
                    warningStatisticsService.addWarningStatistics(warningStatistics);
                }
            } else if ("月".equals(type)) {
                //查统计表中的本月日统计
                Example exampleStatistic = new Example(WarningStatistics.class);
                exampleStatistic.createCriteria().andEqualTo("warningStatisticsType", "日")
                        .andGreaterThanOrEqualTo("createTime", DateFormatUtils.format(date, "yyyy-MM"))
                        .andLessThan("createTime", DateFormatUtils.format(DateUtils.addMonths(date, 1), "yyyy-MM"));
                List<WarningStatistics> warningStatisticsList = warningStatisticsMapper.selectByExample(exampleStatistic);
                //赋值
                warningStatistics.setUserId(warningStatisticsList.get(0).getUserId());
                warningStatistics.setWarningStatisticsTime(date);
                warningStatistics.setWarningStatisticsType("月");
                warningStatistics.setWarningStatisticsName("统计名称" + DateFormatUtils.format(date, "yyyyMMddHHmmss"));
                //处理预警统计纬度
                List<String> collect = warningStatisticsList.stream().map(WarningStatistics::getWarningStatisticsLatitude).distinct().collect(Collectors.toList());
                String[] split = collect.toString().trim().split("\\[|\\]|,");
                List<String> collect1 = Arrays.stream(split)
                        .distinct()
                        .map(s -> s.trim()).filter(s -> !StringUtils.isEmpty(s))
                        .collect(Collectors.toList());
                warningStatistics.setWarningStatisticsLatitude(collect1.toString());
                warningStatistics.setWarningStatisticsCount(warningRecordsList.size());
                warningStatistics.setCreateTime(date);
                warningStatisticsService.addWarningStatistics(warningStatistics);
            }
        }
    }

}
```


![](/images/pasted-6.png)



- 在任务类中测试


![](/images/pasted-7.png)


![](/images/pasted-8.png)

## Mybatis中 foreach collection的三种用法

https://blog.csdn.net/qq_37279783/article/details/86690115

foreach元素的属性主要有 item，index，collection，open，separator，close。

```sql
item 表示集合中每一个元素进行迭代时的别名，
index 指定一个名字，用于表示在迭代过程中，每次迭代到的位置，
open 表示该语句以什么开始，
separator 表示在每次进行迭代之间以什么符号作为分隔 符，
close 表示以什么结束。
```

在使用foreach的时候最关键的也是最容易出错的就是collection属性，该属性是必须指定的，但是在不同情况 下，该属性的值是不一样的，主要有一下3种情况：

```sql
1. 如果传入的是单参数且参数类型是一个List的时候，collection属性值为list
2. 如果传入的是单参数且参数类型是一个array数组的时候，collection的属性值为array
3. 如果传入的参数是多个的时候，我们就需要把它们封装成一个Map了，当然单参数也可
```

map，实际上如果你在传入参数的时候，在breast里面也是会把它封装成一个Map的，map的key就是参数名，所以这个时候collection属性值就是传入的List或array对象在自己封装的map里面的key 下面分别来看看上述三种情况的示例代码：

### 1.单参数List的类型

```sql
<select id="dynamicForeachTest" parameterType="java.util.List" resultType="Blog">
           select * from t_blog where id in
        <foreach collection="list" index="index" item="item" open="(" separator="," close=")">
                #{item}       
        </foreach>    
</select>
```

上述collection的值为list，对应的Mapper是这样的 
public List dynamicForeachTest(List ids); 
测试代码：

```csharp
  @Test
      public void dynamicForeachTest() {
          SqlSession session = Util.getSqlSessionFactory().openSession();      
          BlogMapper blogMapper = session.getMapper(BlogMapper.class);
           List ids = new ArrayList();
           ids.add(1);
           ids.add(3);
           ids.add(6);
          List blogs = blogMapper.dynamicForeachTest(ids);
          for (Blog blog : blogs)
              System.out.println(blog);
          session.close();
      }
```

 

### 2.单参数array数组的类型

```sql
<select id="dynamicForeach2Test" parameterType="java.util.ArrayList" resultType="Blog">
     select * from t_blog where id in
     <foreach collection="array" index="index" item="item" open="(" separator="," close=")">
          #{item}
     </foreach>
 </select>     
```

上述collection为array，对应的Mapper代码： 
public List dynamicForeach2Test(int[] ids); 
对应的测试代码：

```java
  @Test
  public void dynamicForeach2Test() {
          SqlSession session = Util.getSqlSessionFactory().openSession();
          BlogMapper blogMapper = session.getMapper(BlogMapper.class);
          int[] ids = new int[] {1,3,6,9};
          List blogs = blogMapper.dynamicForeach2Test(ids);
          for (Blog blog : blogs)
          System.out.println(blog);    
          session.close();
 }
```

 

### 3.自己把参数封装成Map的类型

```sql
<select id="dynamicForeach3Test" parameterType="java.util.HashMap" resultType="Blog">
         select * from t_blog where title like "%"#{title}"%" and id in
          <foreach collection="ids" index="index" item="item" open="(" separator="," close=")">
               #{item}
          </foreach>
</select>
```

上述collection的值为ids，是传入的参数Map的key，对应的Mapper代码： 
public List dynamicForeach3Test(Map params); 
对应测试代码：

```java
@Test
    public void dynamicForeach3Test() {
        SqlSession session = Util.getSqlSessionFactory().openSession();
         BlogMapper blogMapper = session.getMapper(BlogMapper.class);
          final List ids = new ArrayList();
          ids.add(1);
          ids.add(2);
          ids.add(3);
          ids.add(6);
          ids.add(7);
          ids.add(9);
        Map params = new HashMap();
         params.put("ids", ids);
         params.put("title", "中国");
        List blogs = blogMapper.dynamicForeach3Test(params);
         for (Blog blog : blogs)
             System.out.println(blog);
         session.close();
     }
```

 

## MybatisPlus代码生成器

pom依赖

```java
 		<!--mybatisPlus依赖-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
        </dependency>
        <!--mybatisPlus代码生成器-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-generator</artifactId>
            <version>3.4.0</version>
        </dependency>
        <dependency>
            <groupId>org.freemarker</groupId>
            <artifactId>freemarker</artifactId>
            <version>2.3.31</version>
        </dependency>
```

代码生成

```
package com.it.util;

import com.alibaba.excel.util.StringUtils;
import com.baomidou.mybatisplus.core.exceptions.MybatisPlusException;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.InjectionConfig;
import com.baomidou.mybatisplus.generator.config.*;
import com.baomidou.mybatisplus.generator.config.po.TableInfo;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;
import com.baomidou.mybatisplus.generator.engine.FreemarkerTemplateEngine;

import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

/**
 * MP代码生成器
 *
 * @Author CZM
 */
public class CodeGenerator {

    /**
     * <p>
     * 读取控制台内容
     * </p>
     */
    public static String scanner(String tip) {
        Scanner scanner = new Scanner(System.in);
        StringBuilder help = new StringBuilder();
        help.append("请输入" + tip + "：");
        System.out.println(help);
        if (scanner.hasNext()) {
            String ipt = scanner.next();
            if (StringUtils.isNotBlank(ipt)) {
                return ipt;
            }
        }
        throw new MybatisPlusException("请输入正确的" + tip + "！");
    }

    /**
     * 自动生成代码
     */
    public static void main(String[] args) {
        // 代码生成器
        AutoGenerator mpg = new AutoGenerator();
        // TODO 全局配置
        GlobalConfig gc = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");
        // 生成文件的输出目录【默认 D 盘根目录】
        gc.setOutputDir(projectPath + "/blog/src/main/java");
        // 作者
        gc.setAuthor("CZM");
        // 是否打开输出目录
        gc.setOpen(false);
        // controller 命名方式，注意 %s 会自动填充表实体属性
        gc.setControllerName("%sController");
        // service 命名方式
        gc.setServiceName("%sService");
        // serviceImpl 命名方式
        gc.setServiceImplName("%sServiceImpl");
        // mapper 命名方式
        gc.setMapperName("%sMapper");
        // xml 命名方式
        gc.setXmlName("%sMapper");
        // 开启 swagger2 模式
        gc.setSwagger2(true);
        // 是否覆盖已有文件
        gc.setFileOverride(true);
        // 是否开启 ActiveRecord 模式
        gc.setActiveRecord(true);
        // 是否在xml中添加二级缓存配置
        gc.setEnableCache(false);
        // 是否开启 BaseResultMap
        gc.setBaseResultMap(true);
        // XML columList
        gc.setBaseColumnList(false);
        // 全局 相关配置
        mpg.setGlobalConfig(gc);

        // TODO 数据源配置
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://127.0.0.1:3306/zm_blog?useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC&useSSL=true&characterEncoding=UTF-8");
        dsc.setDriverName("com.mysql.cj.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("root");
        mpg.setDataSource(dsc);

        // TODO 包配置
        PackageConfig pc = new PackageConfig();
        // 父包名。如果为空，将下面子包名必须写全部， 否则就只需写子包名
        pc.setParent("com.it");
        // Entity包名
        pc.setEntity("entity");
        // Service包名
        pc.setService("service");
        // Service Impl包名
        pc.setServiceImpl("service.impl");
        mpg.setPackageInfo(pc);

        // TODO 自定义配置
        InjectionConfig cfg = new InjectionConfig() {
            @Override
            public void initMap() {
                // to do nothing
            }
        };
        // 输出文件配置
        List<FileOutConfig> focList = new ArrayList<>();
        focList.add(new FileOutConfig("/templates/mapper.xml.ftl") {
            @Override
            public String outputFile(TableInfo tableInfo) {
                // 自定义输入文件名称
                return projectPath + "/blog/src/main/resources/mapper/" + tableInfo.getEntityName() + "Mapper.xml";
            }
        });
        // 自定义输出文件
        cfg.setFileOutConfigList(focList);
        mpg.setCfg(cfg);
        mpg.setTemplate(new TemplateConfig().setXml(null));

        // TODO 策略配置
        StrategyConfig strategy = new StrategyConfig();
        // 数据库表映射到实体的命名策略，驼峰原则
        strategy.setNaming(NamingStrategy.underline_to_camel);
        // 字数据库表字段映射到实体的命名策略，驼峰原则
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
        // 实体是否生成 serialVersionUID
        strategy.setEntitySerialVersionUID(false);
        // 是否生成实体时，生成字段注解
        strategy.setEntityTableFieldAnnotationEnable(true);
        // 使用lombok
        strategy.setEntityLombokModel(true);
        // 设置逻辑删除键
        strategy.setLogicDeleteFieldName("del_flag");
        // TODO 指定生成的bean的数据库表名
        strategy.setInclude(scanner("表名，多个英文逗号分割").split(","));
        // 驼峰转连字符
        strategy.setControllerMappingHyphenStyle(true);
        mpg.setStrategy(strategy);
        // 选择 freemarker 引擎需要指定如下加，注意 pom 依赖必须有！
        mpg.setTemplateEngine(new FreemarkerTemplateEngine());
        mpg.execute();
    }
}
```

## 获取登录用户id和姓名


![](/images/pasted-9.png)

## list根据list过滤

 ```java
 List<User> resList = list1 .stream().filter(new Predicate<User>() {
             @Override
             public boolean test(User u) {
                 //如根据name过滤
                 for (User user : list2) {
                     if(u.getName().equals(user.getName())){
                         return false;
                     }
                 }
                 return true;
             }
         }).collect(Collectors.toList());
 ```

## Java数字位数不足前面补0

```java
public static void main(String[] args) {
        int num=6; 
        DecimalFormat decimalFormat = new DecimalFormat("000000");
        String numFormat= decimalFormat .format(num);
        System.out.println(numFormat);//打印结果"000006"
    }

```

## java处理mysql中JSON类型的数据

实体类

```java 
import tk.mybatis.mapper.annotation.ColumnType;
import com.psds.credit.common.context.ObjectJsonHandler;
import com.alibaba.fastjson.JSON;
public class A{
	@ColumnType(typeHandler = ObjectJsonHandler.class)
    private JSON dataJson;
}
```

业务类赋值

```java
import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONObject;
....部分代码省略...
Test upData=new Test();
upData.setDataJson((JSON) JSONObject.toJSON(json));

```

## 返回树形结构的数据

### 使用Hutool TreeUtil 实现

```
https://blog.csdn.net/XikYu/article/details/128715929?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_utm_term~default-0-128715929-blog-127101547.pc_relevant_3mothn_strategy_recovery&spm=1001.2101.3001.4242.1&utm_relevant_index=3
```

```java

    /**
     * 树形结构数据
     *
     * @Author: CZM
     * @Param: []
     */
    public List<Tree<String>> findNodeAll() {
        List<IndustryNode> list = industryNodeMapper.selectAll();
        List<IndustryNode> list2 = CollUtil.newArrayList();
        //浅拷贝赋值
        list2.addAll(list);
        // rootId
        String pid = "0";
        //配置
        TreeNodeConfig nodeConfig = new TreeNodeConfig();
        // 自定义属性名 都要默认值的
        //设置ID对应的名称
        nodeConfig.setIdKey("id");
        // 最大递归深度 4级目录
        nodeConfig.setDeep(4);
        //转换器
        List<Tree<String>> treeList = TreeUtil.build(list2, pid, nodeConfig,
                (node, tree) -> {
                    //id
                    tree.setId(node.getId().toString());
                    //名称
                    tree.setName(node.getTagName());
                    //获取父节点id
                    tree.setParentId(node.getParentId().toString());
                });
        for (Tree<String> tree : treeList) {
            setLevel(tree, 1);
        }
        return treeList;
    }

    /**
     * 设置层级
     *
     * @Author: CZM
     * @Param: [tree, level]
     */
    public void setLevel(Tree<String> tree, int level) {
        if (tree == null) {
            return;
        }
        tree.putExtra("level", level);
        List<Tree<String>> children = tree.getChildren();
        if (children != null) {
            for (Tree<String> child : children) {
                setLevel(child, level + 1);
            }
        }
    }

```
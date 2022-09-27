---
title: OpenRoads中国公路超高规范
author: QinDong
date: 2022-09-27 14:29:00 +0800
categories: OpenRoads 道路廊道
tags: 中国 公路 超高 规范
excerpt: 
---
* content
{:toc}

### 中国公路超高规范-路拱为2%时
```
<?xml version="1.0" encoding="utf-8"?>
<OpenRoadsSuperelevation logFile="" xmlns="OpenRoads">
  <Units length="meter" stationRoundingValue="1" crossSlopeRoundingValue="0.01" />
  <CreationByCorridorSettings eSelection="120mkph 10% Max" lSelection="JTG center" designSpeed="120" pivotMethod="Crown" />
  <RuntimeVariables>
    <RuntimeVariable name="S Curves" type="boolean" value="True" overRideInternalOptionVariable="lengthsAreTotalTransition" description="Only Runoff Length" />
  </RuntimeVariables>
  <MaximumERateCalculations>
      <RateTable name="10%">
      <DesignSpeedRateTable speed="120">
        <RateEntry radius="2950" eRate="NC" />
        <RateEntry radius="2080" eRate=".03" />
        <RateEntry radius="1590" eRate=".04" />
        <RateEntry radius="1280" eRate=".05" />
        <RateEntry radius="1070" eRate=".06" />
        <RateEntry radius="910" eRate=".07" />
        <RateEntry radius="790" eRate=".08" />
        <RateEntry radius="680" eRate=".09" />
        <RateEntry radius="570" eRate=".1" />
      </DesignSpeedRateTable>
    </RateTable>
	<RateTable name="8%">
      <DesignSpeedRateTable speed="60">
        <RateEntry radius="870" eRate="NC" />
        <RateEntry radius="590" eRate=".03" />
        <RateEntry radius="430" eRate=".04" />
        <RateEntry radius="320" eRate=".05" />
        <RateEntry radius="240" eRate=".06" />
        <RateEntry radius="170" eRate=".07" />
        <RateEntry radius="125" eRate=".08" />
      </DesignSpeedRateTable>
    </RateTable>  
    <RateEquation name="120mkph 10% Max" equation="eRate">
      <Variable name="SupRate" description="120mkph 10% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="570" outputValue="0.1" />
          <TableEntry inputValue="680" outputValue="0.09" />
          <TableEntry inputValue="790" outputValue="0.08" />
          <TableEntry inputValue="910" outputValue="0.07" />
          <TableEntry inputValue="1070" outputValue="0.06" />
          <TableEntry inputValue="1280" outputValue="0.05" />
          <TableEntry inputValue="1590" outputValue="0.04" />
          <TableEntry inputValue="2080" outputValue="0.03" />
          <TableEntry inputValue="2950" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 5500 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="120" />
      </Speeds>
    </RateEquation>
    <RateEquation name="120mkph 8% Max" equation="eRate">
      <Variable name="SupRate" description="120mkph 8% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="650" outputValue="0.08" />
          <TableEntry inputValue="790" outputValue="0.07" />
          <TableEntry inputValue="980" outputValue="0.06" />
          <TableEntry inputValue="1190" outputValue="0.05" />
          <TableEntry inputValue="1500" outputValue="0.04" />
          <TableEntry inputValue="1990" outputValue="0.03" />
          <TableEntry inputValue="2860" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 5500 ) ? SupRate:InitialCrossSlope" description="eRate" />   
      <Speeds>
        <Speed speed="120" />
      </Speeds>
    </RateEquation>
    <RateEquation name="120mkph 6% Max " equation="eRate">
      <Variable name="SupRate" description="120mkph 6% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="710" outputValue="0.06" />
          <TableEntry inputValue="970" outputValue="0.05" />
          <TableEntry inputValue="1340" outputValue="0.04" />
          <TableEntry inputValue="1840" outputValue="0.03" />
          <TableEntry inputValue="2730" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 5500 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="120" />
      </Speeds>
    </RateEquation>
    <RateEquation name="120mkph snow and frost" equation="eRate">
      <Variable name="SupRate" description="120mkph snow and frost">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="810" outputValue="0.06" />
          <TableEntry inputValue="1070" outputValue="0.05" />
          <TableEntry inputValue="1410" outputValue="0.04" />
          <TableEntry inputValue="1910" outputValue="0.03" />
          <TableEntry inputValue="2780" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 5500 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="120" />
      </Speeds>
    </RateEquation>
    <RateEquation name="100mkph 10% Max" equation="eRate">
      <Variable name="SupRate" description="100mkph 10% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="360" outputValue="0.1" />
          <TableEntry inputValue="450" outputValue="0.09" />
          <TableEntry inputValue="540" outputValue="0.08" />
          <TableEntry inputValue="640" outputValue="0.07" />
          <TableEntry inputValue="760" outputValue="0.06" />
          <TableEntry inputValue="920" outputValue="0.05" />
          <TableEntry inputValue="1160" outputValue="0.04" />
          <TableEntry inputValue="1520" outputValue="0.03" />
          <TableEntry inputValue="2180" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 4000 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="100" />
      </Speeds>
    </RateEquation>
    <RateEquation name="100mkph 8% Max" equation="eRate">
      <Variable name="SupRate" description="100mkph 8% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="400" outputValue="0.08" />
          <TableEntry inputValue="530" outputValue="0.07" />
          <TableEntry inputValue="690" outputValue="0.06" />
          <TableEntry inputValue="860" outputValue="0.05" />
          <TableEntry inputValue="1100" outputValue="0.04" />
          <TableEntry inputValue="1480" outputValue="0.03" />
          <TableEntry inputValue="2150" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 4000 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="100" />
      </Speeds>
    </RateEquation>
    <RateEquation name="100mkph 6% Max " equation="eRate">
      <Variable name="SupRate" description="100mkph 6% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="440" outputValue="0.06" />
          <TableEntry inputValue="630" outputValue="0.05" />
          <TableEntry inputValue="920" outputValue="0.04" />
          <TableEntry inputValue="1320" outputValue="0.03" />
          <TableEntry inputValue="2000" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 4000 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="100" />
      </Speeds>
    </RateEquation>
    <RateEquation name="100mkph snow and frost" equation="eRate">
      <Variable name="SupRate" description="100mkph snow and frost">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="565" outputValue="0.06" />
          <TableEntry inputValue="770" outputValue="0.05" />
          <TableEntry inputValue="1040" outputValue="0.04" />
          <TableEntry inputValue="1410" outputValue="0.03" />
          <TableEntry inputValue="2090" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 4000 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="100" />
      </Speeds>
    </RateEquation>
    <RateEquation name="80mkph 10% Max" equation="eRate">
      <Variable name="SupRate" description="80mkph 10% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="220" outputValue="0.1" />
          <TableEntry inputValue="280" outputValue="0.09" />
          <TableEntry inputValue="340" outputValue="0.08" />
          <TableEntry inputValue="410" outputValue="0.07" />
          <TableEntry inputValue="500" outputValue="0.06" />
          <TableEntry inputValue="610" outputValue="0.05" />
          <TableEntry inputValue="770" outputValue="0.04" />
          <TableEntry inputValue="1020" outputValue="0.03" />
          <TableEntry inputValue="1460" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 2500 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="80" />
      </Speeds>
    </RateEquation>
    <RateEquation name="80mkph 8% Max" equation="eRate">
      <Variable name="SupRate" description="80mkph 8% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="250" outputValue="0.08" />
          <TableEntry inputValue="320" outputValue="0.07" />
          <TableEntry inputValue="420" outputValue="0.06" />
          <TableEntry inputValue="550" outputValue="0.05" />
          <TableEntry inputValue="710" outputValue="0.04" />
          <TableEntry inputValue="960" outputValue="0.03" />
          <TableEntry inputValue="1410" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 2500 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="80" />
      </Speeds>
    </RateEquation>
    <RateEquation name="80mkph 6% Max " equation="eRate">
      <Variable name="SupRate" description="80mkph 6% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="270" outputValue="0.06" />
          <TableEntry inputValue="400" outputValue="0.05" />
          <TableEntry inputValue="600" outputValue="0.04" />
          <TableEntry inputValue="890" outputValue="0.03" />
          <TableEntry inputValue="1360" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 2500 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="80" />
      </Speeds>
    </RateEquation>
    <RateEquation name="80mkph snow and frost" equation="eRate">
      <Variable name="SupRate" description="80mkph snow and frost">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="360" outputValue="0.06" />
          <TableEntry inputValue="490" outputValue="0.05" />
          <TableEntry inputValue="680" outputValue="0.04" />
          <TableEntry inputValue="940" outputValue="0.03" />
          <TableEntry inputValue="1390" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 2500 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="80" />
      </Speeds>
    </RateEquation>
    <RateEquation name="60mkph 10% Max" equation="eRate">
      <Variable name="SupRate" description="60mkph 10% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="115" outputValue="0.1" />
          <TableEntry inputValue="150" outputValue="0.09" />
          <TableEntry inputValue="190" outputValue="0.08" />
          <TableEntry inputValue="240" outputValue="0.07" />
          <TableEntry inputValue="290" outputValue="0.06" />
          <TableEntry inputValue="360" outputValue="0.05" />
          <TableEntry inputValue="470" outputValue="0.04" />
          <TableEntry inputValue="620" outputValue="0.03" />
          <TableEntry inputValue="900" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 1500 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="60" />
      </Speeds>
    </RateEquation>
    <RateEquation name="60mkph 8% Max" equation="eRate">
      <Variable name="SupRate" description="60mkph 8% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="125" outputValue="0.08" />
          <TableEntry inputValue="170" outputValue="0.07" />
          <TableEntry inputValue="240" outputValue="0.06" />
          <TableEntry inputValue="320" outputValue="0.05" />
          <TableEntry inputValue="430" outputValue="0.04" />
          <TableEntry inputValue="590" outputValue="0.03" />
          <TableEntry inputValue="870" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 1500 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="60" />
      </Speeds>
    </RateEquation>
    <RateEquation name="60mkph 6% Max " equation="eRate">
      <Variable name="SupRate" description="60mkph 6% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="135" outputValue="0.06" />
          <TableEntry inputValue="200" outputValue="0.05" />
          <TableEntry inputValue="320" outputValue="0.04" />
          <TableEntry inputValue="500" outputValue="0.03" />
          <TableEntry inputValue="800" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 1500 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="60" />
      </Speeds>
    </RateEquation>
    <RateEquation name="60mkph 4% Max " equation="eRate">
      <Variable name="SupRate" description="60mkph 4% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="150" outputValue="0.04" />
          <TableEntry inputValue="270" outputValue="0.03" />
          <TableEntry inputValue="610" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 1500 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="60" />
      </Speeds>
    </RateEquation>
    <RateEquation name="60mkph snow and frost" equation="eRate">
      <Variable name="SupRate" description="60mkph snow and frost">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="205" outputValue="0.06" />
          <TableEntry inputValue="290" outputValue="0.05" />
          <TableEntry inputValue="410" outputValue="0.04" />
          <TableEntry inputValue="570" outputValue="0.03" />
          <TableEntry inputValue="860" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 1500 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="60" />
      </Speeds>
    </RateEquation>
    <RateEquation name="40mkph 8% Max" equation="eRate">
      <Variable name="SupRate" description="40mkph 8% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="55" outputValue="0.08" />
          <TableEntry inputValue="80" outputValue="0.07" />
          <TableEntry inputValue="120" outputValue="0.06" />
          <TableEntry inputValue="160" outputValue="0.05" />
          <TableEntry inputValue="220" outputValue="0.04" />
          <TableEntry inputValue="310" outputValue="0.03" />
          <TableEntry inputValue="470" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 600 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="40" />
      </Speeds>
    </RateEquation>
    <RateEquation name="40mkph 6% Max" equation="eRate">
      <Variable name="SupRate" description="40mkph 6% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="60" outputValue="0.06" />
          <TableEntry inputValue="90" outputValue="0.05" />
          <TableEntry inputValue="150" outputValue="0.04" />
          <TableEntry inputValue="250" outputValue="0.03" />
          <TableEntry inputValue="410" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 600 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="40" />
      </Speeds>
    </RateEquation>
    <RateEquation name="40mkph 4% Max " equation="eRate">
      <Variable name="SupRate" description="40mkph 4% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="70" outputValue="0.04" />
          <TableEntry inputValue="130" outputValue="0.03" />
          <TableEntry inputValue="330" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 600 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="40" />
      </Speeds>
    </RateEquation>
    <RateEquation name="40mkph 2% Max " equation="eRate">
      <Variable name="SupRate" description="40mkph 2% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="75" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 600 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="40" />
      </Speeds>
    </RateEquation>
    <RateEquation name="40mkph snow and frost" equation="eRate">
      <Variable name="SupRate" description="40mkph snow and frost">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="90" outputValue="0.06" />
          <TableEntry inputValue="130" outputValue="0.05" />
          <TableEntry inputValue="190" outputValue="0.04" />
          <TableEntry inputValue="280" outputValue="0.03" />
          <TableEntry inputValue="430" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 600 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="40" />
      </Speeds>
    </RateEquation>
    <RateEquation name="30mkph 8% Max" equation="eRate">
      <Variable name="SupRate" description="30mkph 8% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="30" outputValue="0.08" />
          <TableEntry inputValue="40" outputValue="0.07" />
          <TableEntry inputValue="60" outputValue="0.06" />
          <TableEntry inputValue="90" outputValue="0.05" />
          <TableEntry inputValue="120" outputValue="0.04" />
          <TableEntry inputValue="170" outputValue="0.03" />
          <TableEntry inputValue="250" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 350 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="30" />
      </Speeds>
    </RateEquation>
    <RateEquation name="30mkph 6% Max" equation="eRate">
      <Variable name="SupRate" description="30mkph 6% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="35" outputValue="0.06" />
          <TableEntry inputValue="50" outputValue="0.05" />
          <TableEntry inputValue="80" outputValue="0.04" />
          <TableEntry inputValue="140" outputValue="0.03" />
          <TableEntry inputValue="230" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 350 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="30" />
      </Speeds>
    </RateEquation>
    <RateEquation name="30mkph 4% Max " equation="eRate">
      <Variable name="SupRate" description="30mkph 4% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="35" outputValue="0.04" />
          <TableEntry inputValue="60" outputValue="0.03" />
          <TableEntry inputValue="150" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 350 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="30" />
      </Speeds>
    </RateEquation>
    <RateEquation name="30mkph 2% Max " equation="eRate">
      <Variable name="SupRate" description="30mkph 2% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="40" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 350 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="30" />
      </Speeds>
    </RateEquation>
    <RateEquation name="30mkph snow and frost" equation="eRate">
      <Variable name="SupRate" description="30mkph snow and frost">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="55" outputValue="0.06" />
          <TableEntry inputValue="90" outputValue="0.05" />
          <TableEntry inputValue="120" outputValue="0.04" />
          <TableEntry inputValue="180" outputValue="0.03" />
          <TableEntry inputValue="270" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 350 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="30" />
      </Speeds>
    </RateEquation>
    <RateEquation name="20mkph 8% Max" equation="eRate">
      <Variable name="SupRate" description="20mkph 8% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="15" outputValue="0.08" />
          <TableEntry inputValue="30" outputValue="0.07" />
          <TableEntry inputValue="40" outputValue="0.06" />
          <TableEntry inputValue="50" outputValue="0.05" />
          <TableEntry inputValue="70" outputValue="0.04" />
          <TableEntry inputValue="90" outputValue="0.03" />
          <TableEntry inputValue="140" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 150 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="20" />
      </Speeds>
    </RateEquation>
    <RateEquation name="20mkph 6% Max" equation="eRate">
      <Variable name="SupRate" description="20mkph 6% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="15" outputValue="0.06" />
          <TableEntry inputValue="30" outputValue="0.05" />
          <TableEntry inputValue="40" outputValue="0.04" />
          <TableEntry inputValue="70" outputValue="0.03" />
          <TableEntry inputValue="110" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 150 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="20" />
      </Speeds>
    </RateEquation>
    <RateEquation name="20mkph 4% Max " equation="eRate">
      <Variable name="SupRate" description="20mkph 4% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="15" outputValue="0.04" />
          <TableEntry inputValue="30" outputValue="0.03" />
          <TableEntry inputValue="70" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 150 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="20" />
      </Speeds>
    </RateEquation>
    <RateEquation name="20mkph 2% Max " equation="eRate">
      <Variable name="SupRate" description="20mkph 2% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="20" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 150 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="20" />
      </Speeds>
    </RateEquation>
    <RateEquation name="20mkph snow and frost" equation="eRate">
      <Variable name="SupRate" description="20mkph snow and frost">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="25" outputValue="0.06" />
          <TableEntry inputValue="40" outputValue="0.05" />
          <TableEntry inputValue="60" outputValue="0.04" />
          <TableEntry inputValue="80" outputValue="0.03" />
          <TableEntry inputValue="120" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 150 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="20" />
      </Speeds>
    </RateEquation>
  </MaximumERateCalculations>
  <TransitionCalculations>
    <TransitionEquation name="JTG center" equation="TransitionLength">
      <Variable name="gradient" description="Delta G (%) from JTG Table 7.5.4">
        <Table inputVariableName="speed" interpolationType="useLowerBound">
          <TableEntry inputValue="20" outputValue="0.01" />
          <TableEntry inputValue="30" outputValue="0.008" />
          <TableEntry inputValue="40" outputValue="0.006666667" />
          <TableEntry inputValue="60" outputValue="0.005714286" />
          <TableEntry inputValue="80" outputValue="0.005" />
          <TableEntry inputValue="100" outputValue="0.004444444" />
          <TableEntry inputValue="120" outputValue="0.004" />
        </Table>
      </Variable>
      <Variable name="TransitionLength" equation="(WidthLane*NRotatedLanes)*(MaxE-InitialCrossSlope)*bw/gradient" description="Calculated transition length" />   
    </TransitionEquation>
    <TransitionEquation name="JTG edge" equation="TransitionLength">
      <Variable name="gradient" description="Delta G (%) from JTG Table 7.5.4">
        <Table inputVariableName="speed" interpolationType="useLowerBound">
          <TableEntry inputValue="20" outputValue="0.02" />
          <TableEntry inputValue="30" outputValue="0.013333333" />
          <TableEntry inputValue="40" outputValue="0.01" />
          <TableEntry inputValue="60" outputValue="0.008" />
          <TableEntry inputValue="80" outputValue="0.006666667" />
          <TableEntry inputValue="100" outputValue="0.005714286" />
          <TableEntry inputValue="120" outputValue="0.005" />
        </Table>
      </Variable>
      <Variable name="TransitionLength" equation="(WidthLane*NRotatedLanes)*(MaxE-InitialCrossSlope)/gradient" description="Calculated transition length" />
    </TransitionEquation>
  </TransitionCalculations>
  <TransitionOptions interpolateTables="false" percentTransitionOnTangent="0.6" useSpiralLength="true" lengthsAreTotalTransition="true" transitionType="Linear" nonLinearCurveLength="0" startInsideLaneRotationWithOutside="false" />
  <RunoutOptions isFixedLength="false" length="0" />
  <CustomKeyStations>
    <Variable name="FivePercentStation" equation=" ReverseCrownStation * 2 +  ReverseCrownStation  / 2" />
    <CustomKeyStation type="Custom" equation="FivePercentStation" crossSlopeEquation="0.05" />
  </CustomKeyStations>
  <TransitionOverlapOptions>
    <ReverseCurveAdjustmentOption adjustmentType="PlanarTransition" minimumTransitionGap="0" />
    <CurveCurveAdjustmentOption adjustmentType="PlanarTransition" minimumTransitionGap="0" />
  </TransitionOverlapOptions>
</OpenRoadsSuperelevation>
```

### 中国公路超高规范-路拱大于2%时
```
<?xml version="1.0" encoding="utf-8"?>
<OpenRoadsSuperelevation logFile="" xmlns="OpenRoads">
  <Units length="meter" stationRoundingValue="1" crossSlopeRoundingValue="0.01" />
  <CreationByCorridorSettings eSelection="120mkph 10% Max" lSelection="JTG center" designSpeed="120" pivotMethod="Crown" />
  <RuntimeVariables>
    <RuntimeVariable name="S Curves" type="boolean" value="True" overRideInternalOptionVariable="lengthsAreTotalTransition" description="Only Runoff Length" />
  </RuntimeVariables>
  <MaximumERateCalculations>
      <RateTable name="10%">
      <DesignSpeedRateTable speed="120">
        <RateEntry radius="7550" eRate="NC" />
        <RateEntry radius="2950" eRate="RC" />
        <RateEntry radius="2080" eRate=".03" />
        <RateEntry radius="1590" eRate=".04" />
        <RateEntry radius="1280" eRate=".05" />
        <RateEntry radius="1070" eRate=".06" />
        <RateEntry radius="910" eRate=".07" />
        <RateEntry radius="790" eRate=".08" />
        <RateEntry radius="680" eRate=".09" />
        <RateEntry radius="570" eRate=".1" />
      </DesignSpeedRateTable>
    </RateTable>
	<RateTable name="8%">
      <DesignSpeedRateTable speed="60">
        <RateEntry radius="1900" eRate="NC" />
        <RateEntry radius="870" eRate="RC" />
        <RateEntry radius="590" eRate=".03" />
        <RateEntry radius="430" eRate=".04" />
        <RateEntry radius="320" eRate=".05" />
        <RateEntry radius="240" eRate=".06" />
        <RateEntry radius="170" eRate=".07" />
        <RateEntry radius="125" eRate=".08" />
      </DesignSpeedRateTable>
    </RateTable>
    <RateEquation name="120mkph 10% Max" equation="eRate">
      <Variable name="SupRate" description="120mkph 10% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="570" outputValue="0.1" />
          <TableEntry inputValue="680" outputValue="0.09" />
          <TableEntry inputValue="790" outputValue="0.08" />
          <TableEntry inputValue="910" outputValue="0.07" />
          <TableEntry inputValue="1070" outputValue="0.06" />
          <TableEntry inputValue="1280" outputValue="0.05" />
          <TableEntry inputValue="1590" outputValue="0.04" />
          <TableEntry inputValue="2080" outputValue="0.03" />
          <TableEntry inputValue="2950" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 7550 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="120" />
      </Speeds>
    </RateEquation>
    <RateEquation name="120mkph 8% Max" equation="eRate">
      <Variable name="SupRate" description="120mkph 8% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="650" outputValue="0.08" />
          <TableEntry inputValue="790" outputValue="0.07" />
          <TableEntry inputValue="980" outputValue="0.06" />
          <TableEntry inputValue="1190" outputValue="0.05" />
          <TableEntry inputValue="1500" outputValue="0.04" />
          <TableEntry inputValue="1990" outputValue="0.03" />
          <TableEntry inputValue="2860" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 7550 ) ? SupRate:InitialCrossSlope" description="eRate" />   
      <Speeds>
        <Speed speed="120" />
      </Speeds>
    </RateEquation>
    <RateEquation name="120mkph 6% Max " equation="eRate">
      <Variable name="SupRate" description="120mkph 6% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="710" outputValue="0.06" />
          <TableEntry inputValue="970" outputValue="0.05" />
          <TableEntry inputValue="1340" outputValue="0.04" />
          <TableEntry inputValue="1840" outputValue="0.03" />
          <TableEntry inputValue="2730" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 7550 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="120" />
      </Speeds>
    </RateEquation>
    <RateEquation name="120mkph snow and frost" equation="eRate">
      <Variable name="SupRate" description="120mkph snow and frost">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="810" outputValue="0.06" />
          <TableEntry inputValue="1070" outputValue="0.05" />
          <TableEntry inputValue="1410" outputValue="0.04" />
          <TableEntry inputValue="1910" outputValue="0.03" />
          <TableEntry inputValue="2780" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 7550 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="120" />
      </Speeds>
    </RateEquation>
    <RateEquation name="100mkph 10% Max" equation="eRate">
      <Variable name="SupRate" description="100mkph 10% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="360" outputValue="0.1" />
          <TableEntry inputValue="450" outputValue="0.09" />
          <TableEntry inputValue="540" outputValue="0.08" />
          <TableEntry inputValue="640" outputValue="0.07" />
          <TableEntry inputValue="760" outputValue="0.06" />
          <TableEntry inputValue="920" outputValue="0.05" />
          <TableEntry inputValue="1160" outputValue="0.04" />
          <TableEntry inputValue="1520" outputValue="0.03" />
          <TableEntry inputValue="2180" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 5250 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="100" />
      </Speeds>
    </RateEquation>
    <RateEquation name="100mkph 8% Max" equation="eRate">
      <Variable name="SupRate" description="100mkph 8% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="400" outputValue="0.08" />
          <TableEntry inputValue="530" outputValue="0.07" />
          <TableEntry inputValue="690" outputValue="0.06" />
          <TableEntry inputValue="860" outputValue="0.05" />
          <TableEntry inputValue="1100" outputValue="0.04" />
          <TableEntry inputValue="1480" outputValue="0.03" />
          <TableEntry inputValue="2150" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 5250 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="100" />
      </Speeds>
    </RateEquation>
    <RateEquation name="100mkph 6% Max " equation="eRate">
      <Variable name="SupRate" description="100mkph 6% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="440" outputValue="0.06" />
          <TableEntry inputValue="630" outputValue="0.05" />
          <TableEntry inputValue="920" outputValue="0.04" />
          <TableEntry inputValue="1320" outputValue="0.03" />
          <TableEntry inputValue="2000" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 5250 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="100" />
      </Speeds>
    </RateEquation>
    <RateEquation name="100mkph snow and frost" equation="eRate">
      <Variable name="SupRate" description="100mkph snow and frost">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="565" outputValue="0.06" />
          <TableEntry inputValue="770" outputValue="0.05" />
          <TableEntry inputValue="1040" outputValue="0.04" />
          <TableEntry inputValue="1410" outputValue="0.03" />
          <TableEntry inputValue="2090" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 5250 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="100" />
      </Speeds>
    </RateEquation>
    <RateEquation name="80mkph 10% Max" equation="eRate">
      <Variable name="SupRate" description="80mkph 10% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="220" outputValue="0.1" />
          <TableEntry inputValue="280" outputValue="0.09" />
          <TableEntry inputValue="340" outputValue="0.08" />
          <TableEntry inputValue="410" outputValue="0.07" />
          <TableEntry inputValue="500" outputValue="0.06" />
          <TableEntry inputValue="610" outputValue="0.05" />
          <TableEntry inputValue="770" outputValue="0.04" />
          <TableEntry inputValue="1020" outputValue="0.03" />
          <TableEntry inputValue="1460" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 3350 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="80" />
      </Speeds>
    </RateEquation>
    <RateEquation name="80mkph 8% Max" equation="eRate">
      <Variable name="SupRate" description="80mkph 8% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="250" outputValue="0.08" />
          <TableEntry inputValue="320" outputValue="0.07" />
          <TableEntry inputValue="420" outputValue="0.06" />
          <TableEntry inputValue="550" outputValue="0.05" />
          <TableEntry inputValue="710" outputValue="0.04" />
          <TableEntry inputValue="960" outputValue="0.03" />
          <TableEntry inputValue="1410" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 3350 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="80" />
      </Speeds>
    </RateEquation>
    <RateEquation name="80mkph 6% Max " equation="eRate">
      <Variable name="SupRate" description="80mkph 6% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="270" outputValue="0.06" />
          <TableEntry inputValue="400" outputValue="0.05" />
          <TableEntry inputValue="600" outputValue="0.04" />
          <TableEntry inputValue="890" outputValue="0.03" />
          <TableEntry inputValue="1360" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 3350 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="80" />
      </Speeds>
    </RateEquation>
    <RateEquation name="80mkph snow and frost" equation="eRate">
      <Variable name="SupRate" description="80mkph snow and frost">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="360" outputValue="0.06" />
          <TableEntry inputValue="490" outputValue="0.05" />
          <TableEntry inputValue="680" outputValue="0.04" />
          <TableEntry inputValue="940" outputValue="0.03" />
          <TableEntry inputValue="1390" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 3350 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="80" />
      </Speeds>
    </RateEquation>
    <RateEquation name="60mkph 10% Max" equation="eRate">
      <Variable name="SupRate" description="60mkph 10% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="115" outputValue="0.1" />
          <TableEntry inputValue="150" outputValue="0.09" />
          <TableEntry inputValue="190" outputValue="0.08" />
          <TableEntry inputValue="240" outputValue="0.07" />
          <TableEntry inputValue="290" outputValue="0.06" />
          <TableEntry inputValue="360" outputValue="0.05" />
          <TableEntry inputValue="470" outputValue="0.04" />
          <TableEntry inputValue="620" outputValue="0.03" />
          <TableEntry inputValue="900" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 1900 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="60" />
      </Speeds>
    </RateEquation>
    <RateEquation name="60mkph 8% Max" equation="eRate">
      <Variable name="SupRate" description="60mkph 8% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="125" outputValue="0.08" />
          <TableEntry inputValue="170" outputValue="0.07" />
          <TableEntry inputValue="240" outputValue="0.06" />
          <TableEntry inputValue="320" outputValue="0.05" />
          <TableEntry inputValue="430" outputValue="0.04" />
          <TableEntry inputValue="590" outputValue="0.03" />
          <TableEntry inputValue="870" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 1900 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="60" />
      </Speeds>
    </RateEquation>
    <RateEquation name="60mkph 6% Max " equation="eRate">
      <Variable name="SupRate" description="60mkph 6% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="135" outputValue="0.06" />
          <TableEntry inputValue="200" outputValue="0.05" />
          <TableEntry inputValue="320" outputValue="0.04" />
          <TableEntry inputValue="500" outputValue="0.03" />
          <TableEntry inputValue="800" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 1900 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="60" />
      </Speeds>
    </RateEquation>
    <RateEquation name="60mkph 4% Max " equation="eRate">
      <Variable name="SupRate" description="60mkph 4% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="150" outputValue="0.04" />
          <TableEntry inputValue="270" outputValue="0.03" />
          <TableEntry inputValue="610" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 1900 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="60" />
      </Speeds>
    </RateEquation>
    <RateEquation name="60mkph snow and frost" equation="eRate">
      <Variable name="SupRate" description="60mkph snow and frost">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="205" outputValue="0.06" />
          <TableEntry inputValue="290" outputValue="0.05" />
          <TableEntry inputValue="410" outputValue="0.04" />
          <TableEntry inputValue="570" outputValue="0.03" />
          <TableEntry inputValue="860" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 1900 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="60" />
      </Speeds>
    </RateEquation>
    <RateEquation name="40mkph 8% Max" equation="eRate">
      <Variable name="SupRate" description="40mkph 8% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="55" outputValue="0.08" />
          <TableEntry inputValue="80" outputValue="0.07" />
          <TableEntry inputValue="120" outputValue="0.06" />
          <TableEntry inputValue="160" outputValue="0.05" />
          <TableEntry inputValue="220" outputValue="0.04" />
          <TableEntry inputValue="310" outputValue="0.03" />
          <TableEntry inputValue="470" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 800 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="40" />
      </Speeds>
    </RateEquation>
    <RateEquation name="40mkph 6% Max" equation="eRate">
      <Variable name="SupRate" description="40mkph 6% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="60" outputValue="0.06" />
          <TableEntry inputValue="90" outputValue="0.05" />
          <TableEntry inputValue="150" outputValue="0.04" />
          <TableEntry inputValue="250" outputValue="0.03" />
          <TableEntry inputValue="410" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 800 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="40" />
      </Speeds>
    </RateEquation>
    <RateEquation name="40mkph 4% Max " equation="eRate">
      <Variable name="SupRate" description="40mkph 4% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="70" outputValue="0.04" />
          <TableEntry inputValue="130" outputValue="0.03" />
          <TableEntry inputValue="330" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 800 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="40" />
      </Speeds>
    </RateEquation>
    <RateEquation name="40mkph 2% Max " equation="eRate">
      <Variable name="SupRate" description="40mkph 2% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="75" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 800 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="40" />
      </Speeds>
    </RateEquation>
    <RateEquation name="40mkph snow and frost" equation="eRate">
      <Variable name="SupRate" description="40mkph snow and frost">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="90" outputValue="0.06" />
          <TableEntry inputValue="130" outputValue="0.05" />
          <TableEntry inputValue="190" outputValue="0.04" />
          <TableEntry inputValue="280" outputValue="0.03" />
          <TableEntry inputValue="430" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 800 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="40" />
      </Speeds>
    </RateEquation>
    <RateEquation name="30mkph 8% Max" equation="eRate">
      <Variable name="SupRate" description="30mkph 8% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="30" outputValue="0.08" />
          <TableEntry inputValue="40" outputValue="0.07" />
          <TableEntry inputValue="60" outputValue="0.06" />
          <TableEntry inputValue="90" outputValue="0.05" />
          <TableEntry inputValue="120" outputValue="0.04" />
          <TableEntry inputValue="170" outputValue="0.03" />
          <TableEntry inputValue="250" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 450 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="30" />
      </Speeds>
    </RateEquation>
    <RateEquation name="30mkph 6% Max" equation="eRate">
      <Variable name="SupRate" description="30mkph 6% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="35" outputValue="0.06" />
          <TableEntry inputValue="50" outputValue="0.05" />
          <TableEntry inputValue="80" outputValue="0.04" />
          <TableEntry inputValue="140" outputValue="0.03" />
          <TableEntry inputValue="230" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 450 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="30" />
      </Speeds>
    </RateEquation>
    <RateEquation name="30mkph 4% Max " equation="eRate">
      <Variable name="SupRate" description="30mkph 4% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="35" outputValue="0.04" />
          <TableEntry inputValue="60" outputValue="0.03" />
          <TableEntry inputValue="150" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 450 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="30" />
      </Speeds>
    </RateEquation>
    <RateEquation name="30mkph 2% Max " equation="eRate">
      <Variable name="SupRate" description="30mkph 2% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="40" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 450 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="30" />
      </Speeds>
    </RateEquation>
    <RateEquation name="30mkph snow and frost" equation="eRate">
      <Variable name="SupRate" description="30mkph snow and frost">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="55" outputValue="0.06" />
          <TableEntry inputValue="90" outputValue="0.05" />
          <TableEntry inputValue="120" outputValue="0.04" />
          <TableEntry inputValue="180" outputValue="0.03" />
          <TableEntry inputValue="270" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 450 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="30" />
      </Speeds>
    </RateEquation>
    <RateEquation name="20mkph 8% Max" equation="eRate">
      <Variable name="SupRate" description="20mkph 8% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="15" outputValue="0.08" />
          <TableEntry inputValue="30" outputValue="0.07" />
          <TableEntry inputValue="40" outputValue="0.06" />
          <TableEntry inputValue="50" outputValue="0.05" />
          <TableEntry inputValue="70" outputValue="0.04" />
          <TableEntry inputValue="90" outputValue="0.03" />
          <TableEntry inputValue="140" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 200 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="20" />
      </Speeds>
    </RateEquation>
    <RateEquation name="20mkph 6% Max" equation="eRate">
      <Variable name="SupRate" description="20mkph 6% Max">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="15" outputValue="0.06" />
          <TableEntry inputValue="30" outputValue="0.05" />
          <TableEntry inputValue="40" outputValue="0.04" />
          <TableEntry inputValue="70" outputValue="0.03" />
          <TableEntry inputValue="110" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 200 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="20" />
      </Speeds>
    </RateEquation>
    <RateEquation name="20mkph 4% Max " equation="eRate">
      <Variable name="SupRate" description="20mkph 4% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="15" outputValue="0.04" />
          <TableEntry inputValue="30" outputValue="0.03" />
          <TableEntry inputValue="70" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 200 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="20" />
      </Speeds>
    </RateEquation>
    <RateEquation name="20mkph 2% Max " equation="eRate">
      <Variable name="SupRate" description="20mkph 2% Max ">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="20" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 200 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="20" />
      </Speeds>
    </RateEquation>
    <RateEquation name="20mkph snow and frost" equation="eRate">
      <Variable name="SupRate" description="20mkph snow and frost">
        <Table inputVariableName="R" interpolationType="useLowerBound">
          <TableEntry inputValue="25" outputValue="0.06" />
          <TableEntry inputValue="40" outputValue="0.05" />
          <TableEntry inputValue="60" outputValue="0.04" />
          <TableEntry inputValue="80" outputValue="0.03" />
          <TableEntry inputValue="120" outputValue="0.02" />
        </Table>
      </Variable>
   <Variable name="R" equation="abs(Radius)" description="R" />
   <Variable name="eRate" equation="if( R&lt; 200 ) ? SupRate:InitialCrossSlope" description="eRate" />
      <Speeds>
        <Speed speed="20" />
      </Speeds>
    </RateEquation>
  </MaximumERateCalculations>
  <TransitionCalculations>
    <TransitionEquation name="JTG center" equation="if( InitialTransitionLength &lt;=10?10: ceiling( InitialTransitionLength  / 5)  * 5)">
      <Variable name="InitialTransitionLength" equation="(WidthLane*NRotatedLanes)*(MaxE-InitialCrossSlope)/gradient" description="Calculated transition length" />
      <Variable name="gradient" description="Delta G (%) from JTG Table 7.5.4">
        <Table inputVariableName="speed" interpolationType="useLowerBound">
          <TableEntry inputValue="20" outputValue="0.01" />
          <TableEntry inputValue="30" outputValue="0.008" />
          <TableEntry inputValue="40" outputValue="0.006666667" />
          <TableEntry inputValue="60" outputValue="0.005714286" />
          <TableEntry inputValue="80" outputValue="0.005" />
          <TableEntry inputValue="100" outputValue="0.004444444" />
          <TableEntry inputValue="120" outputValue="0.004" />
        </Table>
      </Variable>
      <Variable name="TransitionLength" equation="if( InitialTransitionLength &lt;=10?10: ceiling( InitialTransitionLength  / 5)  * 5)" />
    </TransitionEquation>
    <TransitionEquation name="JTG edge" equation="TransitionLength">
      <Variable name="gradient" description="Delta G (%) from JTG Table 7.5.4">
        <Table inputVariableName="speed" interpolationType="useLowerBound">
          <TableEntry inputValue="20" outputValue="0.02" />
          <TableEntry inputValue="30" outputValue="0.013333333" />
          <TableEntry inputValue="40" outputValue="0.01" />
          <TableEntry inputValue="60" outputValue="0.008" />
          <TableEntry inputValue="80" outputValue="0.006666667" />
          <TableEntry inputValue="100" outputValue="0.005714286" />
          <TableEntry inputValue="120" outputValue="0.005" />
        </Table>
      </Variable>
      <Variable name="InitialTransitionLength" equation="(WidthLane*NRotatedLanes)*(MaxE-InitialCrossSlope)/gradient" description="Calculated transition length" />
      <Variable name="TransitionLength" equation="if( InitialTransitionLength &lt;=10?10: ceiling( InitialTransitionLength  / 5)  * 5)" />
    </TransitionEquation>
  </TransitionCalculations>
  <TransitionOptions interpolateTables="false" percentTransitionOnTangent="0.6" useSpiralLength="true" lengthsAreTotalTransition="true" transitionType="Linear" nonLinearCurveLength="0" startInsideLaneRotationWithOutside="false" />
  <RunoutOptions isFixedLength="false" length="0" />
  <CustomKeyStations>
    <Variable name="FivePercentStation" equation=" ReverseCrownStation * 2 +  ReverseCrownStation  / 2" />
    <CustomKeyStation type="Custom" equation="FivePercentStation" crossSlopeEquation="0.05" />
  </CustomKeyStations>
  <TransitionOverlapOptions>
    <ReverseCurveAdjustmentOption adjustmentType="PlanarTransition" minimumTransitionGap="0" />
    <CurveCurveAdjustmentOption adjustmentType="PlanarTransition" minimumTransitionGap="0" />
  </TransitionOverlapOptions>
</OpenRoadsSuperelevation>
```
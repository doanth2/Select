# Select

import React, { useEffect, useState } from 'react';
import { styled } from '@mui/system';
import { FormControl, InputLabel, NativeSelect, Select, MenuItem  } from '@mui/material';
import { SelectProps } from './Type';

const StyleFrom = styled("form") ({
  minWidth: 130
});

export const InputSelect = <T extends {[key:string]:string| number | null}>(props:SelectProps<T>):JSX.Element => {
  const {list,title,indx,column,name,value,unqkey,defaultValue,onChange,options} = props;
  const [selectedValue,setSelectedValue] = React.useState(defaultValue)
  const nonSelected = typeof(defaultValue) === 'string'? '' : 0

  const handleOnChange = (rowData:string) => {
    setSelectedValue(rowData)
    if(list.length === 0) { return }
    console.log(props)
    onChange(indx,column,typeof(defaultValue) === 'string'? rowData : Number(rowData))
  }

  return(
    <FormControl>
      <InputLabel shrink>{title}</InputLabel>
        <StyleFrom>
          <NativeSelect 
            required
            inputProps={{
              name: 'kbn',
              id: 'uncontrolled-native',
            }}
            onChange={e => {handleOnChange(e.target.value as string)}}
            value={String(selectedValue)}
          >
            {options.blank? <option key={0} value={nonSelected}>{''}</option> : ''}
            {
              list?
                list.map((row) => {
                  return(
                    <option key={row[unqkey]} value={String(row[value])}>{row[name]}</option>)
                })
              : <option key={-1} value={0}>{'読込失敗'}</option>
            }
            {options.all? <option key={99} value={nonSelected}>全て</option> : ''}
          </NativeSelect >
        </StyleFrom>
      </FormControl>
  )
};

----
type select
-----

export interface Option  {
  blank: boolean,
  all: boolean
};

export interface SelectProps<T extends {[key:string]:string|number|null}> {
  list:T[],
  title:string,
  indx:string,
  column:string,
  name:string,
  value:string,
  unqkey:string,
  defaultValue:string | number,
  onChange:(key:string,column:string,value:string | number) => void,
  options:Option
};


